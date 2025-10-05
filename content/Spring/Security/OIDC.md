tags: [[Spring Security]], [[OAuth2.0]]

#### **What is OIDC?**

- **OIDC = OpenID Connect**
- It’s an **identity layer** built on top of **OAuth 2.0**.
- While **OAuth 2.0** is about **authorization** (what an app can do on your behalf),  
    **OIDC is about authentication** (proving _who you are_).
- It uses **JSON Web Tokens (JWTs)** to carry identity information securely.

#### **What are the key components in OIDC?**

- **End User** → The person trying to log in.
- **Relying Party (RP)** → The app or website that wants to know who the user is.
- **OpenID Provider (OP)** → The identity provider (Google, Microsoft, etc.) that authenticates the user.
- **ID Token** → A JWT that proves the user’s identity (contains claims like `sub`, `email`, `name`).
- **Access Token** → (Optional) lets the RP call APIs on behalf of the user.
- **UserInfo Endpoint** → An API at the OP that returns more profile details if requested.

#### **How OIDC Works?**

![[Pasted image 20251005205548.png]]

#### **What are the parameters used in OIDC flows?**

1. `state` — _Session Integrity & CSRF Protection_
	**Why it's used:**
	- Prevents **Cross-Site Request Forgery (CSRF)**.
	- Ensures the **authorization response** matches the **original request** initiated by the client.
	**How it works:**
	- The RP generates a **random string** (`state`) and stores it in the user’s session.
	- It sends this `state` in the **authorization request**.
	- When the OP redirects back with the **authorization code**, it includes the same `state`.
	- The RP compares the returned `state` with the stored one. If they don’t match, the response is rejected.
	**Threat it mitigates:**
	- **CSRF**: An attacker tricks the user into clicking a malicious link that initiates a login flow. Without `state`, the RP might accept a code that wasn’t meant for this session.

2. `nonce` — _Replay Protection for ID Tokens_
	**Why it's used:**
	- Prevents **replay attacks** on **ID Tokens**.
	- Ensures the ID Token was issued for a **specific login attempt**.
	**How it works:**
	- The RP generates a **random string** (`nonce`) and sends it in the **authorization request**.
	- The OP includes this `nonce` in the **ID Token**.
	- The RP checks that the `nonce` in the ID Token matches the one it sent.
	**Threat it mitigates:**
	- **Replay attacks**: An attacker captures a valid ID Token and reuses it to impersonate a user. The `nonce` ensures the token is tied to a specific session.

3. `code_challenge` / `code_verifier` — _PKCE (Proof Key for Code Exchange)_
	**Why it's used:**
	- Prevents **authorization code interception**.
	- Ensures only the app that initiated the login can redeem the code.
	**How it works:**
	- The RP generates a **secret string** (`code_verifier`) and hashes it to create a `code_challenge`.
	- It sends the `code_challenge` in the **authorization request**.
	- Later, in the **token request**, it sends the original `code_verifier`.
	- The OP hashes the `code_verifier` and compares it to the stored `code_challenge`.
	**Threat it mitigates:**
	- **Code interception**: In public clients (like SPAs or mobile apps), an attacker might steal the authorization code during redirect. PKCE ensures they can’t redeem it without the `code_verifier`.

**Summary:**

| Parameter        | Purpose                    | Used In                          | Protects Against  |
| ---------------- | -------------------------- | -------------------------------- | ----------------- |
| `state`          | Session integrity          | Authorization request            | CSRF              |
| `nonce`          | Token freshness            | Authorization request + ID Token | Replay attacks    |
| `code_challenge` | Bind code to client        | Authorization request            | Code interception |
| `code_verifier`  | Prove possession of secret | Token request                    | Code interception |

#### **What are the benefits of OIDC?**

- **Single Sign-On (SSO)** → One login works across multiple apps.
- **No password sharing** → Apps never see your password, only tokens.
- **Interoperability** → Works across web, mobile, and APIs.
- **Extensible** → Supports logout, discovery, dynamic client registration.
