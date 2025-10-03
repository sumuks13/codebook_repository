tags: [[Spring Security]], [[OIDC]]

#### **What is OAuth 2.0?**

- OAuth 2.0 is the industry-standard protocol for authorization.
- It allows applications to obtain limited access to user resources on another service without exposing the user’s credentials.
- In simple terms, it lets you give an app a token to access your data, instead of handing over your full password.

#### **Why is OAuth 2.0 important?**

- Provides **secure delegated access** without sharing passwords.
- Allows **fine-grained control** using scopes (e.g., read-only vs. full access).
- Enables **Single Sign-On (SSO)** with providers like Google, Microsoft, or Facebook.
- Reduces risk by using **short-lived tokens** instead of long-term credentials.
- Supports **different flows** for web apps, mobile apps, and machine-to-machine communication.

#### **What are the main components of OAuth 2.0?**

- **Resource Owner** → The user who owns the data.
- **Client** → The application requesting access.
- **Authorization Server** → Issues tokens after authenticating the user.
- **Resource Server** → Hosts the protected resources and validates tokens.

#### **What are OAuth 2.0 Tokens?**

- **Access Token** → Short-lived credential used to access resources.
- **Refresh Token** → Long-lived credential used to get new access tokens.
- **ID Token** (from OpenID Connect) → Contains user identity information.

#### **What are OAuth 2.0 Flows (Grant Types)?**

- **Authorization Code Flow** → Most secure, used by web & mobile apps.
- **Authorization Code with PKCE** → Secure flow for mobile/native apps.
- **Client Credentials Flow** → For server-to-server (no user involved).
- **Device Code Flow** → For devices without browsers (TVs, IoT).
- **Implicit & Password Flows** → Legacy, now discouraged due to security risks.

#### **Explain Authorization Code Flow**

- **Definition:** A two-step authorization flow where the client receives an authorization code via redirect, then exchanges it for tokens on the backend.
- **When to use:** Confidential clients with a secure backend (web apps). Suitable for SPAs when paired with PKCE.
- **How it works:**
    - User is redirected to `/authorize` with `response_type=code`, `client_id`, `redirect_uri`, `scope`, `state`.
    - After login/consent, the authorization server redirects back with `code` (and `state`).
    - Backend posts to `/token` with `grant_type=authorization_code`, `code`, `redirect_uri`, and client authentication.
    - Server returns `access_token` (+ optional `refresh_token`, `id_token` if OIDC).
- **Security notes:** Tokens stay off the front-end; CSRF mitigated via `state`; use HTTPS; rotate secrets; prefer short-lived access tokens.

![[Pasted image 20251002205545.png]]

#### **Explain Authorization Code Flow with PKCE**

- **Definition:** Authorization Code Flow enhanced with Proof Key for Code Exchange to protect public clients that cannot keep a client secret.
- **When to use:** Mobile apps, native apps, SPAs, CLI apps—any public client.
- **How it works (sequence):**
    - Client generates `code_verifier` and derives `code_challenge` (`S256` recommended).
    - Redirect to `/authorize` with `response_type=code`, `code_challenge`, `code_challenge_method`.
    - After login/consent, receive `code`.
    - Exchange at `/token` with `grant_type=authorization_code`, `code`, `code_verifier` (no client secret required for public clients).
    - Receive tokens as usual.
- **Security notes:** Thwarts code interception/replay; always use `S256` over `plain`; still use `state` and HTTPS.

![[Pasted image 20251003211728.png]]

#### **Explain Client Credentials Flow

- **Definition:** Direct token issuance to a client based on its own credentials, no end-user involved.
- **When to use:** Machine-to-machine (service-to-service) calls, backend jobs, daemons.
- **How it works (sequence):**
    - Client posts to `/token` with `grant_type=client_credentials`, `client_id` and `client_secret` (or mutual TLS/private key JWT).
    - Authorization server returns `access_token`.
    - Client calls resource server with `Authorization: Bearer <token>`.
- **Security notes:** Use strong client auth (mTLS or signed JWT); scope tokens tightly to specific APIs; avoid refresh tokens; keep lifetimes short.

![[Pasted image 20251002210645.png]]

#### **Explain Device Code Flow**

- **Definition:** A flow for devices without a browser or with limited input, using user code verification on a secondary device.
- **When to use:** TVs, consoles, IoT, kiosks.
- **How it works (sequence):**
    - Device requests `device_code` and `user_code` from the device authorization endpoint.
    - Device displays `user_code` and `verification_uri` (and `verification_uri_complete`).
    - User visits the URI on a phone/PC, logs in, and enters the code to consent.
    - Device polls the token endpoint with `grant_type=urn:ietf:params:oauth:grant-type:device_code` and `device_code` until authorized or expired.
    - Authorization server returns `access_token` (and optionally `refresh_token`).
- **Security notes:** Respect polling interval; show expiry; bind scopes minimally; secure device storage; handle denial/timeout gracefully.

![[Pasted image 20251002211032.png]]

#### **Explain Implicit Flow**

- **Definition:** A legacy browser-based flow where the authorization server returns tokens directly in the redirect (URL fragment) without a code exchange.
- **When to use:** Generally avoid; superseded by Authorization Code with PKCE for SPAs.
- **How it works (sequence):**
    - Redirect to `/authorize` with `response_type=token` (or `id_token token` for OIDC).
    - After login/consent, receive `access_token` in the URL fragment.
    - Client parses fragment and calls APIs.
- **Security notes:** Token appears in browser history and referrers; vulnerable to interception; no refresh tokens; prefer code + PKCE.

![[Pasted image 20251002211627.png]]

#### **Explain Resource owner password credentials (ROPC)**

- **Definition:** A legacy flow where the user’s username and password are sent directly to the client to obtain tokens.
- **When to use:** Avoid in modern systems; only for tightly controlled, trusted legacy environments with no viable alternative.
- **How it works (sequence):**
    - Client posts to `/token` with `grant_type=password`, `username`, `password`, optional `scope`, and client authentication.
    - Authorization server returns `access_token` (optionally `refresh_token`).
    - Client accesses resources with the token.
- **Security notes:** Breaks delegated auth principle; exposes credentials to client; difficult to enforce MFA; migrate to code + PKCE or device code.

![[Pasted image 20251002212023.png]]

#### **What are the common parameters and headers used across flows?**

- **Authorize request:** `response_type`, `client_id`, `redirect_uri`, `scope`, `state`, `code_challenge`, `code_challenge_method`.
- **Token request:** `grant_type`, `code`, `redirect_uri`, `client_id`/client auth, `code_verifier`, `device_code`, `username`/`password` (ROPC).
- **HTTP header:** `Authorization: Bearer <access_token>` for resource calls.
- **Token response fields:** `access_token`, `token_type`, `expires_in`, `refresh_token` (optional), `id_token` (OIDC).