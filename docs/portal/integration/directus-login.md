# Authentication (Directus)

This chapter describes how each tool should authenticate users using **Directus credentials**.

We cover two runtime scenarios:

1. **Service environment** (internal network / Docker): eg. Directus reachable as `http://directus:8055`.

2. **External environment** (outside the service): Directus reachable via `https://textailes.athenarc.gr`

## Directus base URL (`DIRECTUS_URL`)

**Do not hardcode** URLs in code. Use an environment variable.

### Service environment (internal)
Inside Docker / internal network, use the Directus service name, eg:

- `DIRECTUS_URL=http://directus:8055`

### External environment (public)
Outside Docker, use the public host:

- `DIRECTUS_URL=https://textailes.athenarc.gr`

## Authentication flow (token-based)

### Step 1 — Login (get access token)
Send credentials to Directus:

**Endpoint:** `POST {DIRECTUS_URL}/auth/login`

**Headers:**
```
Content-Type: application/json
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "secret"
}
```

**Response (success):**
```json
{
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires": 900000,
    "refresh_token": "abc123..."
  }
}
```

### Step 2 — Handle successful authentication

If the response is successful (HTTP 200), the credentials are valid. The tool can then create its own session (example: `login_user(user_instance)`).

**Optional:** If your tool needs to make additional requests to Directus API, extract the `access_token` from the response and include it in subsequent requests:

```
Authorization: Bearer <access_token>
```

## Code Examples

###  Internal Environment Example

If you only need to verify credentials without making further Directus API calls:

```python
import requests

DIRECTUS_URL = "http://directus:8055"

resp = requests.post(
    f"{DIRECTUS_URL}/auth/login",
    json={
        "email": username,
        "password": password
    },
    headers={"Content-Type": "application/json"}
)

if resp.ok:
    # Credentials are valid, create your own session
    login_user(user_instance)
```

### External Environment Example

```python
import requests

DIRECTUS_URL = "https://textailes.athenarc.gr"

# Same authentication flow as internal service
resp = requests.post(
    f"{DIRECTUS_URL}/auth/login",
    json={
        "email": username,
        "password": password
    },
    headers={"Content-Type": "application/json"}
)

if resp.ok:
    login_user(user_instance)
```