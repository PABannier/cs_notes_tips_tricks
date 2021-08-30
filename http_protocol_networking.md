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

Currently studying. Notes will arrive later.

### Redirection

Currently studying. Notes will arrive later.

### Cookies

Currently studying. Notes will arrive later.

### Negotiating HTTP content

Currently studying. Notes will arrive later.

### Conditional requests

Currently studying. Notes will arrive later.

## Network models

### OSI and TCP/IP

Currently studying. Notes will arrive later.

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
