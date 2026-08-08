The Full Lifecycle of a Browser Request to https://example.com

Introduction

When a user enters "https://example.com" into a web browser and presses Enter, several processes happen before the webpage appears on the screen. The browser must first find the server, establish a connection, send a request, receive a response, and then process the received information to display the webpage.

The main stages are DNS lookup, TCP connection, TLS handshake, HTTP request, server processing, HTTP response, and browser rendering.

1. Entering the URL

The process begins when the user enters "https://example.com" into the browser's address bar.

The URL contains several important parts. "https" tells the browser to communicate using the HTTPS protocol. "example.com" is the domain name of the website that the browser needs to contact.

The browser first checks whether it already has information about the domain or an existing connection that can be reused. If the required information is not available, it continues with DNS resolution.

2. DNS Lookup

DNS stands for Domain Name System. Its main purpose is to translate human-readable domain names into IP addresses that computers can use to locate servers.

The browser needs to find the IP address associated with "example.com". It may first check its own DNS cache and the operating system's cache. If the address is not found there, a DNS query is sent to a DNS resolver, usually provided by the internet service provider or another DNS service.

The DNS resolver finds the appropriate DNS records and returns the IP address of the server hosting the website.

The browser can then use this IP address to establish a connection with the server.

3. Establishing a TCP Connection

After obtaining the server's IP address, the browser establishes a TCP connection with the server.

TCP stands for Transmission Control Protocol. It provides a reliable connection between the client, which is the browser, and the server.

The connection is normally established using the TCP three-way handshake:

- The client sends a SYN packet to the server.
- The server responds with a SYN-ACK packet.
- The client sends an ACK packet back to the server.

After this process is completed, a reliable TCP connection has been established between the browser and the server.

4. TLS Handshake

Because the website uses HTTPS rather than HTTP, the browser must establish a secure TLS connection before sending the HTTP request.

TLS stands for Transport Layer Security. It encrypts communication between the browser and the server so that information sent between them cannot easily be read or modified by third parties.

During the TLS handshake, the browser and server negotiate security settings and establish encryption keys. The browser also verifies the server's digital certificate to make sure it is communicating with the correct website.

Once the TLS handshake is successfully completed, encrypted communication can begin.

5. HTTP Request

The browser can now send an HTTP request through the secure connection.

For a basic request to the website, the browser sends a request similar to:

GET / HTTP/1.1
Host: example.com

The "GET" method tells the server that the browser wants to retrieve a resource. The "/" represents the root resource of the website, while the "Host" header identifies the requested domain.

The browser may also send additional headers containing information such as accepted content types, cookies, and browser details.

Because the connection is HTTPS, the request is encrypted while it travels across the network.

6. Server Processing

The request reaches the server associated with "example.com".

The server examines the request and determines what response should be returned. It may locate a requested file, execute application logic, access a database, or perform other processing depending on how the website is built.

For a simple website such as "example.com", the server can return the HTML document requested by the browser.

The server then prepares an HTTP response containing a status code, response headers, and the requested content.

7. HTTP Response

The server sends the HTTP response back to the browser through the established secure connection.

A successful response normally contains a status code such as "200 OK", indicating that the request was successfully processed.

A simplified response can be represented as:

HTTP/1.1 200 OK
Content-Type: text/html

The response also contains the HTML content of the webpage.

The browser receives the response and decrypts it using the secure TLS connection. It can then begin processing the returned content.

8. Browser Rendering

After receiving the HTML document, the browser begins rendering the webpage.

The browser parses the HTML and builds a Document Object Model (DOM), which represents the structure of the webpage.

If the HTML references other resources such as CSS, JavaScript, images, fonts, or other files, the browser may make additional requests to retrieve those resources.

The browser processes the CSS to determine how elements should appear and executes JavaScript when necessary. It then calculates the layout of the page and paints the visual elements onto the screen.

Finally, the user sees the rendered webpage.

9. Complete Lifecycle

The complete lifecycle can therefore be summarized as:

URL entered → DNS lookup → IP address obtained → TCP connection → TLS handshake → HTTP request → server processing → HTTP response → browser parsing → browser rendering → webpage displayed.

Each stage has a specific purpose. DNS helps locate the server, TCP provides reliable communication, TLS provides security, HTTP defines how the browser and server communicate, and the browser converts the returned web resources into the webpage that the user sees.

Conclusion

A webpage request involves much more than simply entering a URL into a browser. Several networking and web technologies work together to deliver the requested content.

For "https://example.com", the browser first resolves the domain name using DNS. It then establishes a TCP connection and performs a TLS handshake to create a secure connection. The browser sends an HTTP request, the server processes the request and returns an HTTP response, and the browser finally processes and renders the received resources.

Understanding this lifecycle provides a foundation for understanding how web applications, APIs, servers, and backend systems communicate over the internet.