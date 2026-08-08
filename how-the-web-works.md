# Lifecycle of a Browser Request to https://example.com

A step-by-step trace of everything that happens between typing `https://example.com` into a browser and seeing the page rendered on screen.

---

## 1. URL Parsing

- Browser parses the input: scheme (`https`), host (`example.com`), port (default `443`), path (`/`).
- Browser checks internal caches (HSTS list, service worker cache, HTTP cache) before doing any network work.

## 2. DNS Lookup

1. **Browser cache** — checked first for a recent A/AAAA record for `example.com`.
2. **OS cache** — if not found, the OS resolver cache is checked.
3. **Router/ISP resolver cache** — next hop if still not found.
4. **Recursive resolver query** — if no cache hits, a recursive DNS resolver (e.g., ISP's or a public one like 8.8.8.8) is queried.
5. **Root → TLD → Authoritative** — the resolver walks the DNS hierarchy: root servers point to `.com` TLD servers, which point to the authoritative name servers for `example.com`, which return the IP address (A record for IPv4 or AAAA for IPv6).
6. Result is cached at each level (browser, OS, resolver) according to the record's TTL.

**Output:** An IP address, e.g. `93.184.216.34`.

## 3. TCP Connection (Transport Layer)

- Browser opens a TCP socket to the resolved IP on port 443.
- **Three-way handshake:**
  1. Client → Server: `SYN`
  2. Server → Client: `SYN-ACK`
  3. Client → Server: `ACK`
- Connection is now established at the transport layer.

## 4. TLS Handshake (Since This Is HTTPS)

1. **ClientHello** — client sends supported TLS versions, cipher suites, and a random value.
2. **ServerHello** — server responds with chosen cipher suite, its digital certificate (containing its public key), and a random value.
3. **Certificate validation** — browser verifies the certificate chain against trusted root CAs, checks expiration, hostname match, and revocation status.
4. **Key exchange** — client and server derive a shared session key (commonly via ECDHE for forward secrecy).
5. **Finished messages** — both sides confirm the handshake, and all further communication is encrypted symmetrically.

Modern TLS 1.3 collapses this into fewer round trips than older TLS 1.2.

## 5. HTTP Request

Once the encrypted channel is ready, the browser sends an HTTP request, e.g.:

```
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 ...
Accept: text/html,application/xhtml+xml,...
Accept-Encoding: gzip, br
Connection: keep-alive
```

- If HTTP/2 or HTTP/3 is negotiated (via ALPN during the TLS handshake), requests may be multiplexed over a single connection rather than using plain HTTP/1.1 framing.

## 6. Server Processing

- The request hits the server (for `example.com`, a simple static server maintained by IANA for documentation/example purposes).
- Server resolves the requested resource, applies any routing logic, checks caching headers (`If-Modified-Since`, `ETag`), and prepares a response.
- For dynamic sites this stage would include application logic, database queries, and template rendering — `example.com` simply serves a static HTML file.

## 7. HTTP Response

Server sends back a response, e.g.:

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1256
Cache-Control: max-age=604800
...

<!doctype html>
<html>
<head><title>Example Domain</title></head>
<body>...</body>
</html>
```

- Status line, headers, then the body (the actual HTML document).
- Connection may stay open (`keep-alive`) for further requests (CSS, images, scripts — though `example.com` has none).

## 8. Browser Rendering Pipeline

1. **HTML Parsing** — browser tokenizes and parses HTML into a **DOM tree** incrementally as bytes arrive.
2. **CSSOM Construction** — any linked or inline CSS is parsed into a **CSSOM tree** (none for `example.com`, so this step is trivial here).
3. **Render Tree** — DOM + CSSOM are combined into a render tree containing only visible elements with computed styles.
4. **Layout (Reflow)** — browser computes exact size and position of each element on the page.
5. **Paint** — browser fills in pixels for text, colors, borders, images onto layers.
6. **Compositing** — layers are combined and drawn to the screen by the GPU/compositor.

## 9. Page Complete

- Browser fires `DOMContentLoaded` once HTML parsing and DOM construction finish.
- `load` event fires once all subresources (images, styles, scripts) have finished loading — for `example.com` this is nearly immediate since there are no external resources.
- Connection is either kept alive for reuse or closed after an idle timeout.

---

## Summary Table

| Stage | Layer | Key Actors |
|---|---|---|
| DNS Lookup | Application/Network | Browser, OS resolver, recursive resolver, root/TLD/authoritative servers |
| TCP Handshake | Transport | Client OS, Server OS |
| TLS Handshake | Transport/Security | Browser, Server, Certificate Authorities |
| HTTP Request/Response | Application | Browser, Web Server |
| Server Processing | Application | Web Server |
| Rendering | Browser Engine | HTML/CSS parser, Layout engine, Compositor |