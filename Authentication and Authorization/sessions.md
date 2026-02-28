# Sessions in Web Applications

## What Is a Session?

A **session** is a temporary, server-side record that stores information about a user across multiple HTTP requests. It acts as the system's memory of who you are and what you're doing, persisting between page loads and interactions.

### The Problem Sessions Solve

**HTTP is stateless** — by design, each request a browser sends to a server is completely independent. The server has no built-in memory of previous requests. Without sessions, you'd have to log in on every single page you visit.

```
Without Sessions:                    With Sessions:
─────────────────                    ──────────────
GET /home   → Who are you?           GET /home   → Cookie: session=abc → Welcome back, John
GET /profile → Who are you?          GET /profile → Cookie: session=abc → Welcome back, John
GET /orders  → Who are you?          GET /orders  → Cookie: session=abc → Welcome back, John
```

Sessions bridge the gap, giving stateless HTTP a sense of continuity.

---

## How Sessions Work — Step by Step

```
┌─────────────────────────────────────────────────────────────────┐
│  Step 1: User submits login credentials                         │
│          username: john_doe | password: ••••••••                │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 2: Server authenticates the user                          │
│          Checks credentials against the database                │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 3: Server creates a Session                               │
│          Stores user data server-side with a unique Session ID  │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 4: Server sends Session ID to the browser via cookie      │
│          Set-Cookie: session_id=abc123xyz; HttpOnly; Secure     │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 5: Browser stores the cookie automatically                │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 6: Every future request includes the Session ID cookie    │
│          GET /dashboard | Cookie: session_id=abc123xyz          │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│  Step 7: Server looks up the Session ID → identifies the user   │
│          Then checks permissions (Authorization)                │
└────────────────────────────┬────────────────────────────────────┘
                             ↓
                      Access Granted or Denied
```

---

## What Does a Session Store?

The server-side session object typically contains:

```json
{
  "session_id": "abc123xyz789",
  "user_id": 42,
  "username": "john_doe",
  "role": "admin",
  "email": "john@example.com",
  "created_at": "2026-02-28T10:00:00Z",
  "last_active": "2026-02-28T10:45:00Z",
  "expires_at": "2026-02-28T11:00:00Z"
}
```

> **Important:** Only the **Session ID** is sent to the browser — never the actual session data. The sensitive data stays on the server.

---

## Session Storage Options

Where the server stores session data depends on the application's needs:

| Storage Type | Description | Pros | Cons |
|---|---|---|---|
| **In-Memory** | Stored in server RAM | Fastest | Lost on server restart, doesn't scale |
| **Database** | Stored in SQL/NoSQL DB | Persistent, scalable | Slower than memory |
| **Redis / Cache** | Stored in a cache server | Fast + persistent | Requires extra infrastructure |
| **File System** | Stored in server files | Simple setup | Slow, hard to scale |

**Redis** is the most common choice for production applications due to its balance of speed and persistence.

---

## Session Cookies

The Session ID is delivered to the browser via a **cookie**. A well-configured session cookie looks like this:

```
Set-Cookie: session_id=abc123xyz; 
            HttpOnly; 
            Secure; 
            SameSite=Strict; 
            Max-Age=3600; 
            Path=/
```

### Cookie Attributes Explained

| Attribute | Purpose |
|---|---|
| **HttpOnly** | JavaScript cannot read the cookie — prevents XSS theft |
| **Secure** | Cookie only sent over HTTPS — prevents interception |
| **SameSite=Strict** | Cookie not sent with cross-site requests — prevents CSRF |
| **Max-Age** | How long (in seconds) the cookie lives |
| **Path** | Which URL paths the cookie applies to |

---

## Session Lifecycle

```
[User Logs In]
      ↓
[Session Created] ──────────────────────────────────┐
      ↓                                              │
[Active Use]                                         │
      ↓                                              │
      ├── User is active → Last active time updated  │
      │                                              │
      └── Idle too long → Idle Timeout               │
                               ↓                     │
                         [Session Destroyed] ←───────┘
                               ↓              (also: logout, absolute expiry)
                         [Cookie Cleared]
                               ↓
                      [User Must Re-login]
```

### Types of Session Expiry

- **Idle Timeout** — Session expires after a period of inactivity (e.g., 30 minutes of no requests)
- **Absolute Expiry** — Session expires after a fixed time regardless of activity (e.g., 8 hours)
- **Manual Logout** — User explicitly ends the session
- **Server Restart** — Clears all in-memory sessions

---

## Sessions vs JWT Tokens

Sessions are not the only approach to maintaining state. **JSON Web Tokens (JWT)** are a popular alternative, especially for APIs and mobile apps.

| Feature | Sessions | JWT Tokens |
|---|---|---|
| **Where data is stored** | Server | Client (browser/app) |
| **Scalability** | Harder — all servers must share session store | Easy — no server storage needed |
| **Revocation** | Easy — delete the session | Hard — token valid until it expires |
| **Size** | Small (just an ID in cookie) | Larger (encoded data in token) |
| **Security control** | Higher server-side control | Less control once issued |
| **Best for** | Traditional web apps | APIs, microservices, mobile apps |

---

## Sessions in Authentication & Authorization

Sessions serve as the **bridge** between the two security concepts:

```
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│ AUTHENTICATION  │      │    SESSION       │      │ AUTHORIZATION   │
│                 │      │                 │      │                 │
│ "Prove who      │  →   │ "I'll remember  │  →   │ "Based on who   │
│  you are"       │      │  you for this   │      │  you are, here's│
│  (done once     │      │  session"       │      │  what you can   │
│  at login)      │      │                 │      │  access"        │
└─────────────────┘      └─────────────────┘      └─────────────────┘
```

Once a session is established, every request is both **identified** (who are you?) and **authorized** (what can you do?) without requiring the user to log in again.

---

## Security Risks & Prevention

### 1. Session Hijacking
An attacker steals the Session ID (via network sniffing or XSS) and impersonates the user.

**Prevention:**
- Use HTTPS at all times
- Set `HttpOnly` and `Secure` cookie flags
- Regenerate the Session ID after login

### 2. Session Fixation
An attacker sets a known Session ID before the user logs in, then uses it after authentication.

**Prevention:**
- Always generate a **brand new Session ID** upon successful login
- Never accept Session IDs from URL parameters

### 3. Cross-Site Request Forgery (CSRF)
A malicious site tricks the browser into sending a request using the user's valid session cookie.

**Prevention:**
- Use `SameSite=Strict` cookies
- Include **CSRF tokens** in forms and validate them server-side

### 4. Session Expiry Not Enforced
Long-lived or never-expiring sessions are dangerous if a device is lost or stolen.

**Prevention:**
- Enforce both **idle timeout** and **absolute expiry**
- Provide users with the ability to view and revoke active sessions

### 5. Insecure Session Storage Client-Side
Storing session data in `localStorage` or `sessionStorage` exposes it to JavaScript (XSS attacks).

**Prevention:**
- Always use **HttpOnly cookies** for session IDs, never localStorage

---

## Best Practices Summary

- Generate session IDs using a **cryptographically secure random generator**
- Always **regenerate the Session ID** after login to prevent fixation attacks
- Store session data **server-side only** — never in the cookie itself
- Use `HttpOnly`, `Secure`, and `SameSite` cookie attributes
- Enforce both **idle and absolute** session expiry
- Invalidate sessions immediately on **logout**
- Use **HTTPS** for all traffic to prevent session ID interception
- Allow users to **view and revoke** all active sessions (like Google's device management)

---

## Summary

> A session is a server-side mechanism that gives stateless HTTP a memory. It stores who the user is after login, passes their identity across every request via a secure cookie, and is automatically destroyed when they log out or the session expires. Sessions are the foundation of user-aware web applications — they make authentication a one-time event and enable authorization on every subsequent request.
