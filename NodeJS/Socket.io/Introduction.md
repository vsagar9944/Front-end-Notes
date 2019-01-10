https://medium.freecodecamp.org/node-package-manager-npm-explained-by-directing-a-movie-9c90f1d16d33	

https://blog.codeanalogies.com/2018/04/07/front-end-v-back-end-explained-by-waiting-tables-at-a-restaurant/



# WebScoket API

The **WebSocket API** is an advanced technology that makes it possible to open a two-way interactive communication session between the user's browser and a server. With this API, you can send messages to a server and receive event-driven responses without having to poll the server for a reply.



## Interfaces

[`WebSocket`](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

The primary interface for connecting to a WebSocket server and then sending and receiving data on the connection.

[CloseEvent](https://developer.mozilla.org/en-US/docs/Web/API/CloseEvent)

The event sent by the WebSocket object when the connection closes.

[`MessageEvent`](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent)

The event sent by the WebSocket object when a message is received from the server.



## Protocal Handshake Headers

Sec-WebSocket-Key

Upgrade



Sec-WebSocket-Accept

Sec-WebSocket-Protocol



## Tools[Section](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API#Tools)

- [HumbleNet](https://hacks.mozilla.org/2017/06/introducing-humblenet-a-cross-platform-networking-library-that-works-in-the-browser/): A cross-platform networking library that works in the browser. It consists of a C wrapper around WebSockets and WebRTC that abstracts away cross-browser differences, facilitating the creation of multi-user networking functionality for games and other apps.
- [µWebSockets](https://github.com/uWebSockets/uWebSockets): Highly scalable WebSocket server and client implementation for [C++11](https://isocpp.org/) and [Node.js](http://nodejs.org/).
- [ClusterWS](https://github.com/ClusterWS/ClusterWS):  Lightweight, fast and powerful framework for building scalable WebSocket applications in [Node.js](http://nodejs.org/).
- [Socket.IO](http://socket.io/): A long polling/WebSocket based third party transfer protocol for [Node.js](http://nodejs.org/).
- [SocketCluster](http://socketcluster.io/): A pub/sub WebSocket framework for [Node.js](http://nodejs.org/) with a focus on scalability.
- [WebSocket-Node](https://github.com/Worlize/WebSocket-Node): A WebSocket server API implementation for [Node.js](http://nodejs.org/).
- [Total.js](http://www.totaljs.com/): Web application framework for [Node.js](http://www.nodejs.org/) (Example: [WebSocket chat](https://github.com/totaljs/examples/tree/master/websocket))
- [Faye](https://www.npmjs.com/package/faye-websocket): A [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) (two-ways connections) and [EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource/) (one-way connections) for [Node.js](http://nodejs.org/) Server and Client.
- [SignalR](http://signalr.net/): SignalR will use WebSockets under the covers when it's available, and gracefully fallback to other techniques and technologies when it isn't, while your application code stays the same.
- [Caddy](https://caddyserver.com/docs/websocket): A web server capable of proxying arbitrary commands (stdin/stdout) as a websocket.
- [ws](https://github.com/websockets/ws): a popular WebSocket client & server library for [Node.js](https://nodejs.org/en/).
- [jsonrpc-bidirectional](https://github.com/bigstepinc/jsonrpc-bidirectional): Asynchronous RPC which, on a single connection, may have functions exported on the server and, and the same time, on the client (client may call server, server may also call client).