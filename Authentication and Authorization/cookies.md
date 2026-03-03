# Cookies 

## 1. Definition

A cookie is a small piece of data stored in the user's browser by the server.

The browser automatically sends cookies back to the server with every request to the same domain.

Cookies help the server recognize users and maintain state in HTTP.

---

## 2. Why Cookies Exist

HTTP is stateless.

This means the server does not remember previous requests.

Cookies solve this by allowing the browser to store data and send it automatically with future requests.

Without cookies:

Request 1 → Server forgets  
Request 2 → Server forgets  

With cookies:

Request 1 → Server sets cookie  
Request 2 → Browser sends cookie → Server recognizes user  

---

## 3. How Cookies Work

### Step 1: Server sets cookie

Server response:

```
Set-Cookie: sessionId=abc123
```

Browser stores the cookie.

---

### Step 2: Browser sends cookie automatically

Next request:

```
Cookie: sessionId=abc123
```

Server receives cookie and identifies the user.

---

## 4. Where Cookies Are Stored

Cookies are stored in the browser.

You can view them in:

DevTools → Application → Cookies

Each cookie contains:

- Name
- Value
- Domain
- Expiration
- Security attributes

---

## 5. Cookie Structure

Basic format:

```
Set-Cookie: name=value
```

Example:

```
Set-Cookie: sessionId=abc123
```

---

## 6. Important Cookie Attributes

### HttpOnly

```
HttpOnly
```

Prevents JavaScript from accessing the cookie.

Protects against XSS attacks.

Used for authentication cookies.

---

### Secure

```
Secure
```

Cookie is sent only over HTTPS.

Prevents interception over HTTP.

---

### Max-Age / Expires

```
Max-Age=3600
```

Cookie expires after 1 hour.

---

### SameSite

```
SameSite=Strict
```

Controls whether cookie is sent with cross-site requests.

Prevents CSRF attacks.

Values:
- Strict
- Lax
- None

---

## 7. Types of Cookies

### Session Cookies

Temporary.

Deleted when browser closes.

No expiration time set.

---

### Persistent Cookies

Stored for a defined time.

Example:

```
Max-Age=86400
```

Stored for 1 day.

---

## 8. Cookies and Authentication

Cookies are not authentication methods.

They are only storage and transport mechanisms.

They can store:

- Session ID
- JWT token
- User preferences
- Tracking data

---

## 9. Sessions + Cookies

Server stores session data:

```
sessionId → userId
```

Server sends:

```
Set-Cookie: sessionId=abc123
```

Browser sends it automatically.

Server looks up session in database.

Stateful authentication.

---

## 10. JWT + Cookies

Server creates JWT.

Server sends:

```
Set-Cookie: token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

Browser sends token automatically.

Server verifies signature.

Stateless authentication.

---

## 11. Cookies vs LocalStorage

Cookies:
- Automatically sent with requests
- Small size (~4KB)
- Can be secured with HttpOnly and Secure

LocalStorage:
- Not sent automatically
- Must be sent manually via headers
- Accessible by JavaScript

---

## 12. Security Best Practices

- Use HttpOnly for authentication cookies
- Use Secure (HTTPS only)
- Use SameSite to prevent CSRF
- Avoid storing sensitive data directly in cookies

---

## 13. Core Understanding

Cookie = storage + automatic transport mechanism.

It does NOT:

- Authenticate users
- Validate identity
- Check permissions

It simply carries data between browser and server.

---

## 14. Summary

A cookie is a small browser-stored key-value pair that is automatically sent with HTTP requests to maintain state and identify users.

Cookies enable stateful communication over the stateless HTTP protocol.