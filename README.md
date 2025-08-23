# Go Rate Limiter Examples

This repository demonstrates three different approaches to implementing rate limiting in Go web applications:

- **Token Bucket (using `golang.org/x/time/rate`)**
- **Per-Client Rate Limiting (IP-based, using `golang.org/x/time/rate`)**
- **Third-Party Middleware (`tollbooth`)**

---

## Project Structure

```
RateLimiter/
├── per-client-rate-limiting/
│   ├── main.go
│   ├── go.mod
│   └── go.sum
├── token-bucket/
│   ├── main.go
│   ├── limit.go
│   ├── go.mod
│   └── go.sum
└── tollbooth/
    ├── main.go
    ├── go.mod
    └── go.sum
```

---

## 1. Token Bucket

**Folder:** `token-bucket/`

Implements a simple global rate limiter using the token bucket algorithm.

- **Rate:** 2 requests/second, burst up to 4.
- All clients share the same limiter.

**Run:**

```sh
cd token-bucket
go run main.go
```

---

## 2. Per-Client Rate Limiting

**Folder:** `per-client-rate-limiting/`

Implements rate limiting per client IP address.

- **Rate:** 2 requests/second, burst up to 4 per client.
- Each client (IP) has its own limiter.

**Run:**

```sh
cd per-client-rate-limiting
go run main.go
```

---

## 3. Tollbooth Middleware

**Folder:** `tollbooth/`

Uses the [tollbooth](https://github.com/didip/tollbooth) middleware for rate limiting.

- **Rate:** 1 request/second (configurable).
- Easy to use and configure.

**Run:**

```sh
cd tollbooth
go run main.go
```

---

## Testing Rate Limiting

After starting any server, you can test rate limiting by sending multiple requests quickly:

**Using curl (Windows Command Prompt):**

```sh
for /l %i in (1,1,10) do curl http://localhost:8080/ping
```

**Using PowerShell:**

```sh
1..10 | ForEach-Object { Invoke-WebRequest http://localhost:8080/ping }
```

**Expected Behavior:**

- The first few requests should succeed (`HTTP 200`).
- Once the rate limit is exceeded, you should receive `HTTP 429 Too Many Requests` with a JSON error message.

---

## Things I Learned

- **How to implement rate limiting in Go** using both standard libraries (`golang.org/x/time/rate`) and third-party middleware (`tollbooth`).
- **Difference between global and per-client rate limiting** and how to manage state for each client using Go maps and mutexes.
- **How to use Go’s `sync.Mutex`** to safely handle concurrent access to shared resources.
- **How to use Go routines** for background cleanup tasks (such as removing stale clients).
- **How to handle HTTP requests and responses** in Go, including setting headers and encoding JSON responses.
- **How to test rate limiting** using command-line tools like `curl` and PowerShell.
- **How to structure a Go project** with multiple approaches and keep code modular and organized.

---

## License

MIT
