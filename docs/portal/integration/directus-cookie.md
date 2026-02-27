# Cookie-Based Authentication Flow

This chapter describes how tools in the Textailes toolbox handle authentication using **cookie-based sessions** with the centralized portal login.

## Overview

When a user attempts to access a tool from the toolbox that requires authentication, the tool automatically redirects the user to the **Portal Login Page**. After successful authentication, the user is redirected back to the tool with a validated session cookie.

This approach provides a **seamless Single Sign-On (SSO) experience** across all tools in the ecosystem.

## How It Works

The authentication system uses a **Directus refresh token** stored in a cookie named `textailes_refresh_token`. 

**Key characteristics:**
- **Domain-wide sharing**: Cookie uses domain `.textailes.athenarc.gr`, making it accessible to all tools
- **Automatic validation**: Tools validate tokens on each request using `POST /auth/refresh`
- **Short-lived access tokens**: Each request gets a fresh access token for API calls

## Authentication Flow

### 1. User accesses a protected tool

When a user tries to access a tool that requires authentication:
- The tool checks for the `textailes_refresh_token` cookie
- If missing or invalid, the tool redirects to the Portal Login

### 2. Redirect to Portal Login

The tool redirects the user to the Portal Login page with a `redirect_url` parameter:

```
https://textailes.athenarc.gr/archive/user/login?redirect_url=<TOOL_URL>
```

**URL Parameters:**
- `redirect_url` - The URL of the tool where the user should be redirected after successful login

### 3. Portal Login checks for existing session

**Endpoint:** `POST {DIRECTUS_URL}/auth/refresh`

**Request Body:**
```json
{
  "refresh_token": "<token_from_cookie>"
}
```

**Scenario A — Valid Refresh Token:**
- Portal finds valid `textailes_refresh_token` cookie
- Calls `/auth/refresh` to validate token
- If valid, portal automatically redirects to `redirect_url` without showing login form
- Cookie is already set, so tool can use it immediately

**Scenario B — No Valid Token:**
- User sees the login form
- User enters credentials (email + password)
- Portal calls `/auth/login` and receives refresh token
- Portal sets `textailes_refresh_token` cookie with domain `.textailes.athenarc.gr`
- User is redirected to `redirect_url`

### 4. Tool validates the token

When the user returns to the tool:
1. Tool reads `textailes_refresh_token` from cookies
2. Tool calls `POST /auth/refresh` with the token in request body
3. Directus responds with:
   ```json
   {
     "data": {
       "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
       "expires": 900000,
       "refresh_token": "new_refresh_token_if_rotated"
     }
   }
   ```
4. Tool stores `access_token` in request context (e.g., Flask's `g` object)
5. If a new `refresh_token` is provided, tool updates the cookie
6. User gains access to protected resources

## Implementation Requirements

### Public Routes

Tools can define public routes that don't require authentication:

```python
PUBLIC_PATHS = {"/health", "/status"}           # Exact matches
PUBLIC_PREFIXES = ("/static/", "/public/")      # Prefix matches
```

### Cookie Configuration

**Cookie Name:** `textailes_refresh_token`

**Cookie Settings:**
```python
resp.set_cookie(
    COOKIE_NAME,
    refresh_token_value,
    httponly=True,        # Prevent JavaScript access
    secure=is_https,          # Only send over HTTPS (production)
    samesite="Lax",       # CSRF protection
    path="/",             # Available site-wide
    domain=".textailes.athenarc.gr"  # Shared across subdomains
)
```

## Complete Implementation Example (Python/Flask)

This is a complete, production-ready example showing how to implement cookie-based authentication in a Flask tool:

```python
from __future__ import annotations
import os
import requests
from urllib.parse import quote
from flask import request, redirect, g, make_response


# --- Configuration ---
DIRECTUS_URL = "https://textailes.athenarc.gr"
COOKIE_NAME = "textailes_refresh_token"
LOGIN_URL = "https://textailes.athenarc.gr/archive/user/login"
APP_BASE = "http://toolExample.textailes.athenarc.gr:0000"  # Your tool's base URL

# Routes that don't require authentication
PUBLIC_PATHS = {"/health"}
PUBLIC_PREFIXES = ("/static/",)


def redirect_to_login():
    """Redirect user to portal login with return URL."""
    # Preserve the full path including query string
    next_path = request.full_path if request.query_string else request.path
    redirect_url = APP_BASE.rstrip("/") + next_path
    return redirect(f"{LOGIN_URL}?redirect_url={quote(redirect_url, safe=':/?=&')}")


def directus_refresh(refresh_token: str):
    """
    Validate refresh token with Directus.
    Returns token data (access_token, refresh_token) or None if invalid.
    """
    try:
        r = requests.post(
            f"{DIRECTUS_URL}/auth/refresh",
            json={"refresh_token": refresh_token},
            timeout=8,
        )
        if r.status_code != 200:
            print("refresh failed:", r.status_code, r.text[:200], flush=True)
            return None
        
        data = r.json()
        return data.get("data") or data
    except Exception as e:
        print("refresh exception:", e, flush=True)
        return None


def init_auth(app):
    """Initialize authentication middleware for Flask app."""
    
    @app.before_request
    def auth_gate():
        """Check authentication before each request."""
        path = request.path
        
        # Skip authentication for public routes
        if path in PUBLIC_PATHS or any(path.startswith(p) for p in PUBLIC_PREFIXES):
            return None
        
        # Check for refresh token cookie
        refresh_token = request.cookies.get(COOKIE_NAME)
        if not refresh_token:
            return redirect_to_login()
        
        # Validate token with Directus
        tokens = directus_refresh(refresh_token)
        if not tokens:
            return redirect_to_login()
        
        # Store access token for this request
        g.access_token = tokens.get("access_token")
        
        # Check if we received a new refresh token (token rotation)
        new_refresh = tokens.get("refresh_token")
        if new_refresh and new_refresh != refresh_token:
            g.new_refresh_token = new_refresh
        else:
            g.new_refresh_token = None
        
        return None
    
    @app.after_request
    def apply_refresh_cookie(resp):
        """Update refresh token cookie if rotated."""
        new_refresh = getattr(g, "new_refresh_token", None)
        if not new_refresh:
            return resp
        
        # Determine cookie domain
        host = request.host.split(":")[0]
        domain = ".textailes.athenarc.gr" if host.endswith("textailes.athenarc.gr") else None
        
        # Check if connection is secure
        is_https = request.is_secure or request.headers.get("X-Forwarded-Proto") == "https"
        
        # Update cookie with new refresh token
        resp.set_cookie(
            COOKIE_NAME,
            new_refresh,
            httponly=True,
            secure=is_https,
            samesite="Lax",
            path="/",
            domain=domain,
        )
        return resp

```