---
title: Socket.io
---

#Introduction


Socket.IO is a library that enables real-time, bidirectional and event-based communication between the browser and the serve

[Socket.io docs](https://socket.io/docs/v4)


<br />

## Before socket.io

### Pre-socketio

* Built on top of web sockets api
    * it has fail over mechanisms, in case it doesnt use web sockets
* Uses tcp

[networking cool image](https://es.wikipedia.org/wiki/Internet#/media/Archivo:Internet_map_1024.jpg)


<br />

## Events

### Ping

### Pong

### emit

### on

### connect

<br />


## Namespaces

* `.of('namespacename')` specifies a namespace name.
    * `io.of('/').emit('message')` is shorthand `io.emit('message')`
    * `io.of('/').on('message')` is shorthand `io.on('message')`
* on client we use `io('http://localhost:9000/namespacename')` to connect to a namespace  

ALL these are server only

Send an event from the server to this socket only:
```javascript
socket.emit()
socket.send()
```


Send an event from a socket to a room:

NOTE: remember, this will not go to the sending socket

```javascript
socket.to(roomName).emit()
socket.in(roomName).emit()
```



Because each socket has it's own room, named by it's socket.id, a socket can send a message to another socket:

```javascript
socket.to(anotherSocketId).emit('hey');
socket.in(anotherSocketId).emit('hey');
```


A namespace can send a message to any room:

```javascript
io.of(aNamespace).to(roomName).emit()
io.of(aNamespace).in(roomName).emit()
```


A namespace can send a message to the entire namespace

```javascript
io.emit()
io.of('/').emit()
io.of('/admin').emit()
```


<br />


## Rooms
