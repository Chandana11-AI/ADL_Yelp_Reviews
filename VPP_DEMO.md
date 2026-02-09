# VPP Demo: Keycloak + FastAPI RBAC (Local)

This guide shows a minimal **VPP-aligned** demo where Keycloak handles login and FastAPI enforces **role-based access control (RBAC)**:

- `/vpp/forecast` → **any authenticated user**
- `/vpp/dispatch` → **operator role only**

---

## 1) Run Keycloak locally (Docker)

Official Keycloak container quickstart:

```bash
docker run -p 8080:8080 \
  -e KEYCLOAK_ADMIN=admin \
  -e KEYCLOAK_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:latest start-dev
```

Open the admin console:

- http://localhost:8080/
- Login: `admin / admin`

---

## 2) Configure Keycloak for the VPP demo

In Keycloak:

1. **Create Realm**: `vpp`
2. **Create Client (OpenID Connect)**:
   - Client ID: `vpp-api`
   - Keep defaults for dev
3. **Create Realm Roles**:
   - `operator`
   - `analyst`
4. **Create Users**:
   - `lyra-operator` → assign role `operator`
   - `lyra-analyst` → assign role `analyst`
5. *(Optional)* Enable self-registration:
   - Realm Settings → Login → **User registration** ON

> Tip: For automation later, Keycloak can import a realm JSON at startup using `--import-realm`.

---

## 3) FastAPI: verify Keycloak JWT via OIDC discovery + JWKS

Install dependencies:

```bash
pip install fastapi uvicorn httpx fastapi-jwks
```

Create `main.py`:

```python
from fastapi import FastAPI, Depends, HTTPException
from fastapi_jwks import JWKSAuth, JWKSAuthConfig

REALM = "vpp"
KEYCLOAK_BASE = "http://localhost:8080"
OIDC_DISCOVERY = f"{KEYCLOAK_BASE}/realms/{REALM}/.well-known/openid-configuration"

auth = JWKSAuth(
    JWKSAuthConfig(
        oidc_discovery_url=OIDC_DISCOVERY,
        issuer=f"{KEYCLOAK_BASE}/realms/{REALM}",
        # audience is optional based on your Keycloak client settings
        # audience="vpp-api",
    )
)

app = FastAPI(title="VPP API (Demo)", version="0.1")

def require_role(role: str):
    def _check(user=Depends(auth)):
        realm_roles = (
            user.get("realm_access", {}).get("roles", [])
            if isinstance(user, dict)
            else []
        )
        if role not in realm_roles:
            raise HTTPException(status_code=403, detail=f"Missing role: {role}")
        return user
    return _check

@app.get("/health")
def health():
    return {"status": "ok"}

@app.get("/vpp/forecast")
def forecast(user=Depends(auth)):
    return {
        "who": user.get("preferred_username"),
        "forecast": {"load_kw": [10, 12, 9]},
    }

@app.post("/vpp/dispatch")
def dispatch(user=Depends(require_role("operator"))):
    return {
        "who": user.get("preferred_username"),
        "dispatch": "battery_discharge_approved",
    }
```

Run:

```bash
uvicorn main:app --reload --port 8000
```

Open Swagger:

- http://localhost:8000/docs

---

## 4) Get a token (demo-friendly)

Use OIDC discovery to find the token endpoint:

```
http://localhost:8080/realms/vpp/.well-known/openid-configuration
```

From the discovery doc, read `token_endpoint`, then use Postman or curl to get an access token once.

Paste the access token into Swagger **Authorize** (Bearer) to test `/vpp/forecast` and `/vpp/dispatch`.

---

## 5) References (official + practical)

- Keycloak Docker quickstart: https://www.keycloak.org/getting-started/getting-started-docker
- Keycloak containers + realm import: https://www.keycloak.org/server/containers
- FastAPI security: https://fastapi.tiangolo.com/tutorial/security/
- FastAPI OAuth2/JWT tutorial: https://fastapi.tiangolo.com/tutorial/security/oauth2-jwt/
- fastapi-jwks (GitHub): https://github.com/k4black/fastapi-jwks
- FastAPI + Keycloak discussion: https://github.com/tiangolo/fastapi/discussions/3772

---

## 6) 30-second demo script (VPP-flavored)

> “Keycloak is our Identity Provider. It handles registration/login and issues signed JWT access tokens.  
> Our FastAPI VPP services are resource servers: they validate tokens using the realm’s JWKS keys and enforce role-based access like operator vs analyst.  
> This mirrors real VPP environments where dispatch permissions are tightly controlled.”
