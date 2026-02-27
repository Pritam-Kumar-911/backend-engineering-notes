# Serialization & Deserialization

## What is Serialization?

Serialization is the process of converting data from your program's memory into a format that can be **stored or transmitted**. Your program works with objects and data structures in memory, but memory cannot be sent over a network or saved to a file directly — it needs to be converted into a portable format first.

> Think of it as packing your belongings into a box before shipping them somewhere.

---

## What is Deserialization?

Deserialization is the **reverse** — taking the portable format (like a JSON string) that arrived from somewhere and converting it back into a usable object in your program's memory.

> Think of it as unpacking the box after it arrives at its destination.

---

## How They Work Together

```
Your Program (Object)
        ↓
  [ Serialization ]
        ↓
JSON String (travels over network / saved to file)
        ↓
  [ Deserialization ]
        ↓
Your Program (Object again)
```

---

## Code Example (JavaScript)

### Serialization — Object → JSON String

```js
const user = { name: "Pritam", age: 20 };

const serialized = JSON.stringify(user);
// Result: '{"name":"Pritam","age":20}'
// Now it's a string that can travel over HTTP
```

### Deserialization — JSON String → Object

```js
const jsonString = '{"name":"Pritam","age":20}';

const deserialized = JSON.parse(jsonString);
// Result: { name: "Pritam", age: 20 }
// Now it's a usable object again
```

---

## Real World Connection

You have already been using serialization and deserialization without knowing the terms:

- Your **Weather App** received a JSON response from the OpenWeatherMap API — that response was serialized data. When you used `fetch()` and called `.json()`, you were deserializing it.
- Your **Node.js backend** sends `res.json(data)` — Node is serializing your object into a JSON string before sending it over HTTP.
- Every **API call** you make involves serialization on the sender's side and deserialization on the receiver's side.

---

## Common Serialization Formats

| Format | Used For |
|--------|----------|
| JSON | Web APIs, config files, most common |
| XML | Legacy systems, some enterprise APIs |
| CSV | Spreadsheets, data exports |
| Binary (e.g. Protocol Buffers) | High performance systems, microservices |

---

## Key Takeaway

Serialization = **Object → Portable Format** (for sending/storing)

Deserialization = **Portable Format → Object** (for using)

JSON is the most common format on the web. `JSON.stringify()` serializes. `JSON.parse()` deserializes. Everything else is built on top of this concept.
