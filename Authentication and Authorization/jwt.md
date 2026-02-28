# JSON Web Token (JWT) — Complete Notes

## 1. Definition

JWT (JSON Web Token) is a secure, self-contained way to transmit user identity information between client and server.

It is commonly used for authentication and authorization in backend systems.

JWT is **stateless**, meaning the server does not store session data.

---

## 2. Why JWT exists

Traditional authentication uses sessions:

- Server stores session data
- Requires database or memory lookup
- Hard to scale in distributed systems

JWT solves this by:

- Storing identity inside the token itself
- Server only verifies signature
- No session storage needed

This makes JWT **stateless and scalable**

---

## 3. JWT Structure

JWT has 3 parts:

```
Header.Payload.Signature
```

Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJ1c2VySWQiOjQ1LCJyb2xlIjoidXNlciJ9
.
abc123signature
```

---

## 4. JWT Components

## 4.1 Header

Contains metadata:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

- alg → signing algorithm
- typ → token type

---

## 4.2 Payload

Contains user data (claims):

```json
{
  "userId": 45,
  "role": "admin",
  "exp": 1712345678
}
```

Common claims:

- userId → identifies user
- role → permissions
- exp → expiration time

Payload is NOT encrypted, only encoded.

---

## 4.3 Signature

Created using:

```
Signature = Hash(Header + Payload + SECRET_KEY)
```

Used to verify authenticity.

Prevents tampering.

---

## 5. How JWT Authentication Works

## Step 1: Login

User sends credentials:

```
POST /login
```

Server verifies credentials.

---

## Step 2: JWT Creation

Server creates token:

```json
{
  "userId": 45,
  "role": "user"
}
```

Signs with SECRET_KEY.

Sends JWT to client.

---

## Step 3: Client Stores JWT

Client stores JWT in:

- localStorage
OR
- cookies

---

## Step 4: Client sends JWT in requests

```
Authorization: Bearer <JWT>
```

---

## Step 5: Server verifies JWT

Server:

- verifies signature
- reads payload
- allows or denies access

No database lookup required.

---

## 6. Why JWT is Stateless

Server does NOT store:

- session data
- login state
- user identity

Server only stores:

```
SECRET_KEY
```

All identity info is inside JWT.

Each request is independent.

---

## 7. Stateless vs Stateful Comparison

## Sessions (Stateful)

Server stores:

```
sessionId → user data
```

Requires database lookup.

---

## JWT (Stateless)

Client stores identity.

Server verifies signature only.

No storage needed.

---

## 8. Advantages of JWT

### Scalability
Works across multiple servers easily.

### Performance
No database lookup needed.

### Stateless
No session storage required.

### Microservices friendly
Each service can verify JWT independently.

---

## 9. Disadvantages of JWT

### Cannot easily revoke tokens
Until expiration.

### Secret key compromise is dangerous
Attacker can create valid tokens.

### Payload is visible
Do not store sensitive data.

---

## 10. Security Best Practices

### Use expiration

Example:

```json
{
  "exp": 1712345678
}
```

---

### Keep SECRET_KEY secure

Store in environment variables:

```
JWT_SECRET=mysecretkey
```

Never expose publicly.

---

### Use HTTPS

Prevents token interception.

---

### Use short expiration times

Example:

- Access token → 15 minutes
- Refresh token → longer duration

---

## 11. HS256 vs RS256

HS256:
- Uses single SECRET_KEY
- Simpler
- Less secure at scale

RS256:
- Uses public/private keys
- More secure
- Used by large systems

---

## 12. Real-World Analogy

JWT is like a government ID card.

Server verifies signature like verifying official stamp.

No need to check database.

---

## 13. When to use JWT

Use JWT when building:

- APIs
- Microservices
- Distributed systems
- Scalable applications

---

## 14. Summary

JWT is a stateless authentication mechanism where:

- Client stores identity in token
- Server verifies signature
- No session storage needed

It improves scalability and performance in modern backend systems.