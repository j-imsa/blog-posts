---
title: "OAuth 2.0 & OpenID Connect: Essential Terminology"
date: "2025-10-03"
author: "Iman"
categories: [ "Security", "Authentication" ]
tags: [ "oauth", "oidc", "security", "authentication", "authorization" ]
summary: "A comprehensive dictionary explaining OAuth 2.0 and OpenID Connect terminology, including core concepts, roles, token types, grant flows, and security best practices for implementing modern authentication and authorization in applications."
cover_image: "./img/pexels-cottonbro-8721342.jpg"
---

![pexels-cottonbro-8721342.jpg](img/pexels-cottonbro-8721342.jpg "A person using a laptop with security lock icons displayed, representing digital authentication and security")

# OAuth 2.0 & OpenID Connect Dictionary

A comprehensive guide to understanding OAuth 2.0 and OpenID Connect terminology.

---

## Core Concepts

### OAuth 2.0

> An **authorization framework** that enables applications to obtain limited access to user accounts on an HTTP service.
> It works by delegating user authentication to the service that hosts the user account and authorizing third-party
> applications to access that account.

**Key point:** OAuth is about **authorization**, not authentication.

### OpenID Connect (OIDC)

> An **authentication layer** built on top of OAuth 2.0. While OAuth 2.0 handles authorization, OIDC adds identity
> verification capabilities, making it suitable for authentication scenarios.

**Key point:** OIDC = OAuth 2.0 + Identity

---

## Roles

### Resource Owner

> The **user** who owns the data and can grant access to it. Typically a human end-user who authorizes an application to
> access their account.

### Client

> The **application** requesting access to the user's resources. This could be a web app, mobile app, or server-side
> application.

### Authorization Server

> The server that **authenticates the resource owner** and issues access tokens after getting proper authorization.
> Examples: Auth0, Keycloak, Okta, Google, Microsoft.

### Resource Server

> The server hosting the **protected resources** (APIs). It accepts and validates access tokens before serving the
> requested data.

---

## Client Types

### Confidential Clients

> Applications that can **securely store credentials** (client secret):

- Server-side web applications
- Backend services
- Can use authorization code flow with client secret

### Public Clients

> Applications that **cannot securely store credentials**:

- Single-page applications (SPAs)
- Mobile apps
- Desktop applications
- Must use PKCE (Proof Key for Code Exchange) for security

---

## Tokens

### Access Token

> A **credential** used to access protected resources. It represents the authorization granted to the client.

**Characteristics:**

- Short-lived (typically 5-60 minutes)
- Opaque or JWT format
- Contains scopes defining permissions
- Sent in API requests via `Authorization: Bearer <token>` header

**Example (JWT):**

```json
{
  "sub": "user123",
  "scope": "read:profile write:posts",
  "exp": 1696348800,
  "iss": "https://auth.example.com"
}
```

### Refresh Token

> A **long-lived token** used to obtain new access tokens without re-authenticating the user.

**Characteristics:**

- Long-lived (days, weeks, or months)
- Stored securely by the client
- Can be revoked by the authorization server
- Not sent to resource servers

**Use case:** Maintaining user sessions without requiring repeated logins.

### ID Token (OIDC only)

> A **JWT token** that contains information about the authenticated user. This is specific to OpenID Connect.

**Characteristics:**

- Contains user identity claims (sub, name, email, etc.)
- Signed by the authorization server
- Used by the client to verify user identity
- Should NOT be sent to APIs (use access tokens instead)

**Example:**

```json
{
  "sub": "user123",
  "name": "John Doe",
  "email": "john@example.com",
  "iss": "https://auth.example.com",
  "aud": "my-client-id",
  "exp": 1696348800
}
```

---

## Identity Provider (IdP)

> A service that **stores and manages user identity information** and provides authentication services.

**Examples:**

- Google
- Microsoft Azure AD
- Okta
- Auth0
- Keycloak

**Capabilities:**

- User authentication
- Token issuance
- Single Sign-On (SSO)
- Multi-factor authentication (MFA)

---

## Grant Types

Different flows for obtaining access tokens based on the client type and use case.

### Authorization Code Flow

> The **most secure and recommended** flow for confidential clients (server-side apps).

**Flow:**

1. User is redirected to the authorization server
2. User logs in and grants consent
3. Authorization server redirects back with an authorization code
4. Client exchanges code for access token (server-side)

**Use case:** Traditional web applications

### Authorization Code Flow with PKCE

> Enhanced version for **public clients** (SPAs, mobile apps).

**PKCE (Proof Key for Code Exchange)** adds security by using:

- `code_verifier` – Random string generated by a client
- `code_challenge` – Hashed version of verifier

**Use case:** Single-page applications, mobile apps

### Client Credentials Flow

> **Machine-to-machine authentication** without a user.

**Flow:**

1. Client sends credentials directly to the authorization server
2. Receives access token

**Use case:** Backend services, APIs calling other APIs

### Implicit Flow

> ❌ **Deprecated** – Access token returned directly in URL fragment.

**Security issues:**

- Tokens exposed in browser history
- No refresh token support

**Replaced by:** Authorization Code Flow with PKCE

### Resource Owner Password Credentials (ROPC)

> ❌ **Not recommended** – User provides username/password directly to client.

**Issues:**

- Client has access to user credentials
- Defeats the purpose of OAuth

**Only use when:** Legacy migration scenarios or first-party applications

### Refresh Token Flow

> Used to obtain a new access token using a refresh token.

**Flow:**

1. Client sends refresh token to authorization server
2. Receives new access token (and optionally new refresh token)

---

## Scopes

> Scopes define the **level of access granted** to an access token.

**Examples:**

- `read:profile` – Read user profile
- `write:posts` – Create/update posts
- `openid` – Required for OIDC authentication
- `email` – Access to user's email
- `offline_access` – Request refresh token

---

## Additional Terms

### Consent Screen

> The screen shown to users asking them to **approve or deny** the client's access request.

### Redirect URI (Callback URL)

> The URL where the authorization server redirects the user after authentication. Must be **pre-registered** for
> security.

### State Parameter

> A random value used to **prevent CSRF attacks** by maintaining state between request and callback.

### Nonce

> A random value used in OIDC to **prevent replay attacks**. Included in ID token.


---

## Best Practices

✅ Always use HTTPS in production

✅ Use Authorization Code Flow with PKCE for SPAs

✅ Never store access tokens in localStorage (use secure, httpOnly cookies when possible)

✅ Implement token refresh logic

✅ Validate ID tokens properly (signature, issuer, audience, expiration)

✅ Use short-lived access tokens

✅ Implement proper scope management



---

## References

- [OAuth 2.0 RFC 6749](https://tools.ietf.org/html/rfc6749)
- [OpenID Connect Core Spec](https://openid.net/specs/openid-connect-core-1_0.html)
- [OAuth 2.0 Security Best Practices](https://tools.ietf.org/html/draft-ietf-oauth-security-topics)


