WebSocket is a protocol that provides a two-way, bidirectional (full-duplex) communication over a single TCP connection, so that the data can be transferred simultaneously at any time.  While WebSocket runs over TCP, it is different from other TCP-based full-duplex protocols because, it runs over the standard HTTP/SSL port numbers. WebSocket is primarily designed for server-browser communication and to address limitations of HTTP/1.1 – primarily its inability to perform out-of-order request processing and lack of support for server-initiated transactions (server push).

 

A WebSocket connection is established by a handshake mechanism between the client and the server, whereby both agree to upgrade from HTTP to WebSockets. Though the handshake itself happens using the HTTP protocol, subsequent traffic does not conform to HTTP. In fact, the client and server are free to choose any format for data exchange, including binary, compressed or encrypted.

 

Enable WebSocket for a Service

Perform the following steps to enable WebSocket:

Go to the ADVANCED > System Configuration page.
In the Advanced Settings section, set Show Advanced Settings to Yes and click Save.
Go to the BASIC > Services page.
In the Services section, click Edit next to the service to which you want to enable WebSocket.
In the Service window:
Scroll down to the Advanced Configuration section.
Set Enable WebSocket to Yes.
Click Save.