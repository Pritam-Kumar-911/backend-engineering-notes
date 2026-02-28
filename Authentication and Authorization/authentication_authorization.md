# Authentication & Authorization

## Overview

Two of the most fundamental concepts in application security are **Authentication** and **Authorization**. Though often mentioned together, they serve distinct purposes:

| Concept | Question Answered | Example |
|---|---|---|
| **Authentication** | *Who are you?* | Logging in with a username and password |
| **Authorization** | *What are you allowed to do?* | An admin can delete users; a regular user cannot |

---

## Authentication

### What Is It?

Authentication is the process of **verifying the identity** of a user, system, or application. Before a system grants any access, it needs to confirm that you are who you claim to be.

### Common Authentication Methods

#### 1. Password-Based Authentication
The most traditional method. A user provides a username and password, which the server checks against stored credentials.

```
User → [username + password] → Server → Verify → Grant/Deny Access
```

> **Best Practice:** Passwords should never be stored in plain text. Always use hashing algorithms like **bcrypt** or **Argon2**.

#### 2. Multi-Factor Authentication (MFA)
Requires two or more verification factors:

- **Something you know** — password, PIN
- **Something you have** — phone (OTP), hardware token
- **Something you are** — fingerprint, face scan (biometrics)

MFA dramatically reduces risk even if a password is compromised.

#### 3. Token-Based Authentication (JWT)
After login, the server issues a signed **JSON Web Token (JWT)** that the client stores and sends with every request.

```json
{
  "header": { "alg": "HS256", "typ": "JWT" },
  "payload": {
    "user_id": 42,
    "username": "john_doe",
    "role": "admin",
    "exp": 1735689600
  },
  "signature": "abc123..."
}
```

#### 4. OAuth / Social Login
Delegates authentication to a trusted third party (Google, GitHub, Facebook). The user logs in with their existing account, and the third party confirms their identity to your application.

#### 5. Biometric Authentication
Uses physical characteristics — fingerprints, facial recognition, iris scans — increasingly common on mobile devices.

---

### Authentication Flow (Password-Based)

```
1. User submits login form (username + password)
         ↓
2. Server retrieves hashed password from database
         ↓
3. Server hashes submitted password and compares
         ↓
4. Match? → Authentication SUCCESS → Proceed
   No match? → Authentication FAILURE → Deny Access
```

---

### Common Authentication Vulnerabilities

| Vulnerability | Description | Prevention |
|---|---|---|
| **Brute Force** | Trying many password combinations | Rate limiting, account lockout |
| **Credential Stuffing** | Using leaked username/password pairs | MFA, breach detection |
| **Phishing** | Tricking users into revealing credentials | User education, MFA |
| **Man-in-the-Middle** | Intercepting credentials in transit | HTTPS / TLS encryption |

---

## Authorization

### What Is It?

Authorization is the process of **determining what an authenticated user is permitted to do**. It happens *after* authentication and controls access to resources and actions.

### Common Authorization Models

#### 1. Role-Based Access Control (RBAC)
Users are assigned **roles**, and roles have **permissions**. This is the most widely used model.

```
User → assigned → Role → has → Permissions → access → Resources

Example:
  Admin Role    → [read, write, delete, manage_users]
  Editor Role   → [read, write]
  Viewer Role   → [read]
```

#### 2. Attribute-Based Access Control (ABAC)
Access decisions are based on **attributes** of the user, resource, and environment.

```
Example Rule:
  IF user.department == "finance"
  AND resource.type == "financial_report"
  AND time.hour BETWEEN 9 AND 17
  THEN ALLOW access
```

More flexible than RBAC but more complex to implement.

#### 3. Discretionary Access Control (DAC)
The **owner** of a resource decides who can access it. Common in file systems (Linux file permissions).

```
-rwxr-xr-- (owner: read/write/execute, group: read/execute, others: read)
```

#### 4. Mandatory Access Control (MAC)
Access is controlled by a **central authority** based on classifications (e.g., Top Secret, Confidential). Common in military and government systems.

---

### Authorization Flow

```
1. Authenticated request arrives (user identity is known)
         ↓
2. System identifies the resource being requested
         ↓
3. System checks user's role/permissions
         ↓
4. Permission granted? → Allow access
   Permission denied? → Return 403 Forbidden
```

---

### Common Authorization Vulnerabilities

| Vulnerability | Description | Prevention |
|---|---|---|
| **Privilege Escalation** | Gaining higher permissions than allowed | Strict role enforcement |
| **Insecure Direct Object Reference (IDOR)** | Accessing another user's data by changing an ID | Always verify ownership server-side |
| **Missing Function-Level Access Control** | Unprotected admin endpoints | Enforce authorization on every endpoint |

---

## Authentication vs Authorization — Side by Side

```
┌─────────────────────────────────────────────────────────┐
│                    User Requests Access                  │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│               AUTHENTICATION                             │
│         "Verify WHO the user is"                        │
│   Check credentials → Issue token/session               │
└────────────────────────┬────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────┐
│               AUTHORIZATION                              │
│         "Verify WHAT they can do"                       │
│   Check role/permissions → Allow or Deny                │
└────────────────────────┬────────────────────────────────┘
                         ↓
                   Access Granted or Denied
```

---

### Authorization
- Apply the **Principle of Least Privilege** — give users only the permissions they need
- Always enforce authorization checks **server-side**, never trust the client
- Validate that users can only access **their own** data
- Log and monitor access to sensitive resources
- Regularly review and audit permissions and roles

---

## Summary

> **Authentication** confirms identity. **Authorization** enforces what that identity can do. Both are essential layers of security — authentication without authorization means everyone gets access to everything, and authorization without authentication means there's no way to know who to trust.
