# HTTP protocol and networking

The following notes are taken from the excellent introduction to HTTP protocol
course of Pierre Giraud (https://www.pierre-giraud.com/http-reseau-securite-cours/introduction-protocole-transfert-hypertexte/).

## HTTP fundamentals

### Introduction to HTTP

HTTP: HyperText Transfer Protocol

HTTP is a basic communication protocol to exchange data and resources (HTML pages,
images, videos, etc.). It defines a **set of rules** that governs data exchanges.
It essentially defines the syntax of messages exchanged between two machines.

**Client-server relationship**

Client: requests, asks for access to a resource.
Server: responses, answers to the requests emitted by the clients.

HTTP protocol provides a framework to this relationship.

HTTP has consistently evolved over the years but was not initially designed for
performance matters. Two defining (and not very efficient) pillars of HTTP:

- A request can only get one resource at a time.

- HTTP is a stateless protocol. Stateless means that HTTP does not require that the
  server keeps information about the client between two requests. Requests are independent
  from each other.

E.g.: A web browser (client) asks for a HTML web page. Then as it browses it, it gradually
asks to the server additional resources (images, icons, etc), creating additional requests.

**HTTP offers syntactic rules, but how data actually transfers from machine A to machine B?**

HTTP is a subset of a much larger infrastructure called a **network model**.

- OSI (Open System Interconnection)
- TCP/IP

Those two models define layers, each containing its own protocol. OSI is a more exhaustive model
than TCP/IP, but TCP/IP offers a much simpler way to implement or modify different layers of the
network model. **HTTP** is a transfer protocol of the application layer of the OSI and TCP/IP models.

![OSI and TCP/IP](images/http_protocol_networking/osi_tcp_ip.png)

**HTTP and HTTPS**

SSL: Secure Sockets Layer
It offers an encrypted protocol to exchange data. It is now called TLS (Transport
Layer Security).

HTTPS is a secured variant of HTTP that encapsulates the TLS protocol.

<span style="color:red">HTTP uses the port 80 by default. HTTPS uses the port 443.</span>

### Requests, responses and sessions

Two-step process: the client sends requests to the server, the server sends back responses to
the clients.

The format used by the HTTP protocol is:

- A starting line describing the request and the way it is processed.
- HTTP headers.
- One blank line.
- The message body.

The starting line contains the method (e.g. POST, GET...), the target (e.g. a URL)
and the HTTP version which specifies the expected version of the response.

The headers usually contains the size of the request, the language and enables the
support for cookies.

Several kind of requests exist depending on what the client want to execute. **GET** is
used to retrieve a resource, while **POST** is used to send data to the server. **DELETE**
is used to indicate the deletion of a resource.

A list of HTTP statuses:

- **1xx**: temporary responses
- **2xx**: success
- **3xx**: redirection
- **4xx**: client-side error
- **5xx**: server-side error

This cycle requests/responses unfolds in a larger context called a **HTTP session**.

**A session indicates all the exchanges that occurs between the connection and the
disconnection by the server**.

The connection is dealt at the transport layer level in the TCP or the OSI model.
HTTP does not deal with connecting the client and the server.

### The evolution of the HTTP protocol

Different versions of the HTTP protocol: HTTP/0.9, HTTP/1, HTTP/2 and now HTTP/3.

HTTP/2 is inspired from SPDY, a protocol created by Google. HTTP/3 aims at quickly
loading data pages by establishing a hierarchy between the components of a web page
(images, style sheets...).

A **multiplexing system** was implemented: send multiple requests in parallel without
waiting for the responses of each one (non-blocking). Responses are not expected to
come out in the same order as their corresponding requests arrived.

### Caching

Cached data corresponds to the local storage of data previously received following
a HTTP request.

Broadly speaking, a client makes a HTTP request to a server. The server sends a response.
The client (usually the browser) can cache the data. When needing a resource it previously
asked to the server, instead of sending a request, it can simply look for data in its cache.

There exists two kinds of cache: private and shared cache.

Every cache entry has a key and one or multiple HTTP responses. A cache is endowed with a
limited lifetime via the Header `Expires`, giving an explicit expiration date to the cached
data.

### Redirection

Redirection occurs when a request is made by the client to the server; the server sends back
a response with a status code of the form 3xx. The resource the client is looking for has been
moved, the server can't directly response to the request.

Once the response is returned, the client can decide to create a new request to another server
found in the `Location` header of the response.

The most common redirections are 301 and 304.

### Cookies

As stated above, HTTP is a stateless protocol, meaning that two requests/responses are supposed
independent.

A HTTP cookie is a small piece of data that a server sends to a client in order for the latter
to send it back in future requests (e.g. site preferences, shopping cart). Cookies are useful to
manage sessions.

In practice, once a request is made by the client, a server can send back a response which
contains one or more cookies with a specific HTTP Header `Set-Cookie`.

```
Set-Cookie: SID=14322ac76654aa83; Path=/; Secure; HttpOnly
Set-Cookie: lang=fr-FR; Path=/; Domain=example.com
```

Cookies contain attributes that define their use case, lifetime duration, etc.

## Network models

### OSI and TCP/IP

The advantage of using layers in a network model is to be able to modify one layer
independently from the other. It mitigates the risk of breaking other layers along
the way. 

OSI: 4 layers.
TCP/IP: 7 layers.

![OSI and TCP/IP](images/http_protocol_networking/osi_tcp_ip.png)

- Application: the closest from the user. Most of the protocols (SMTP, FTP, **HTTP**)
are in this layer. This layer purpose is to be establish, synchronise connections between
machines.

- Presentation: to format transmitted  data.

### TCP

Currently studying. Notes will arrive later.

### IP, the Internet protocol

Currently studying. Notes will arrive later.

### The SPDY protocol

Currently studying. Notes will arrive later.

### The QUIC protocol

Currently studying. Notes will arrive later.

### UDP

Currently studying. Notes will arrive later.

## HTTP and security

### HTTP authentification

Currently studying. Notes will arrive later.

### SSL and HTTPS

Currently studying. Notes will arrive later.

### CORS: cross-origin resource sharing

Currently studying. Notes will arrive later.

### Security policy

Currently studying. Notes will arrive later.
