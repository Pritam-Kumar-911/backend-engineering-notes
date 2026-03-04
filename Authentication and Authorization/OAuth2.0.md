# OAuth 2.0 — Complete Notes

## 1. Definition

OAuth 2.0 is an authorization framework that allows a user to grant a third-party application limited access to their resources without sharing their password.

It is defined by the Internet Engineering Task Force (IETF).

OAuth is primarily about authorization, not authentication.

---

## 2. Why OAuth 2.0 Exists

Problem:

Users want to allow one application to access their data in another system without giving away their password.

Example:

"Login with Google"

Instead of sharing your Google password with another website:

- Google authenticates you
- Google issues an access token
- The website uses that token to access limited user data

---

## 3. Core Concept

OAuth allows delegated access using tokens instead of credentials.

Password → never shared  
Access Token → shared with limited scope  

---

## 4. Important Roles in OAuth 2.0

There are four main roles:

### 1. Resource Owner
The user who owns the data.

### 2. Client
The application requesting access to the user's data.

### 3. Authorization Server
The server that authenticates the user and issues access tokens.

### 4. Resource Server
The API that holds the user's data.

---

## 5. Authorization Code Flow (Most Common Flow)

This is the most secure and widely used flow for web applications.

### Step 1: User clicks "Login with Provider"

Client redirects user to authorization server:

```
GET /authorize
```

---

### Step 2: User logs in and approves permissions

User authenticates on provider's website.

User grants permission.

---

### Step 3: Authorization Server returns authorization code

User is redirected back to client:

```
https://yourapp.com/callback?code=abc123
```

Authorization code is temporary.

---

### Step 4: Client exchanges code for access token

Client sends:

```
POST /token
```

Authorization server responds:

```json
{
  "access_token": "xyz123",
  "expires_in": 3600
}
```

---

### Step 5: Client uses access token

Client calls resource server:

```
Authorization: Bearer xyz123
```

Resource server verifies token and returns data.

---

## 6. Access Token

Access token:

- Represents user authorization
- Has expiration time
- Can have limited scope
- Does not contain user password

Access tokens are often implemented using JWT.

---

## 7. Scope

Scope defines what access is granted.

Examples:

- read_profile
- read_email
- write_data

User explicitly approves scopes.

---

## 8. OAuth vs Authentication

OAuth was designed for authorization.

Authentication verifies identity.

For login systems, OAuth is often combined with OpenID Connect (OIDC).

---

## 9. Common OAuth Flows

### Authorization Code Flow
Used in web apps (most common and secure).

### Client Credentials Flow
Used for server-to-server communication.

### Device Flow
Used for devices with limited input (e.g., smart TVs).

### Implicit Flow (Deprecated)
Used in older single-page apps.

---

## 10. Security Benefits

- No password sharing
- Limited access via scopes
- Tokens expire
- Access can be revoked
- Suitable for third-party integrations

---

## 11. Real-World Usage

OAuth is used by major platforms for third-party login and API access.

Common examples include:

- Login with Google
- Login with Facebook
- GitHub API access

---

## 12. OAuth vs Session vs JWT

Session:
- Server stores authentication state
- Used in traditional web applications

JWT:
- Client stores identity in token
- Stateless authentication

OAuth:
- Delegated authorization framework
- Allows one system to access another system securely

---

## 13. High-Level Flow Summary

User → Client → Authorization Server → Token Issued → Resource Server → Data Access

No password sharing occurs between client and resource owner.

---

## 14. Key Takeaways

- OAuth 2.0 is an authorization framework.
- It enables delegated access without password sharing.
- It uses access tokens to grant limited permissions.
- It is widely used for social logins and API integrations.
- It improves security and user control over data sharing.