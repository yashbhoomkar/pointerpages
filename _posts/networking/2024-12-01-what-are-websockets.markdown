---
layout: single
title: "What are WebSockets? Explained Simply"
date: 2024-12-01 09:00:00 +0530
categories:
  - networking
tags:
  - websockets
  - real-time
  - tcp
  - http
  - socket
  - interview
toc: true
classes: wide
---

## 🧠 Introduction

Have you ever used a chat app, live score update site, or real-time collaboration tool like Google Docs?

Chances are, **WebSockets** were involved.

WebSockets allow real-time, two-way communication between a browser (or client) and a server — something traditional HTTP was never designed for.

---

## 🧪 What is a WebSocket?

> A **WebSocket** is a communication protocol that enables **persistent**, **bi-directional**, and **full-duplex** communication between a client and server over a single **TCP** connection.

---

## 🔁 Traditional HTTP vs WebSocket

| Feature              | HTTP                      | WebSocket                     |
|----------------------|---------------------------|-------------------------------|
| Request-Response     | Client sends, server replies | Bi-directional messages       |
| Connection Type      | Short-lived (stateless)   | Persistent                    |
| Overhead             | High (headers in every request) | Low (handshake once)     |
| Real-time Updates    | Requires polling or long-polling | Native real-time             |

---

## 🔧 How WebSocket Works

### Step 1: HTTP Handshake

It starts with a normal HTTP request:

GET /chat HTTP/1.1 Host: server.com Upgrade: websocket Connection: Upgrade

csharp
Copy
Edit

If the server supports WebSockets, it responds with:

HTTP/1.1 101 Switching Protocols Upgrade: websocket Connection: Upgrade

yaml
Copy
Edit

Now the connection is **upgraded to WebSocket**.

---

### Step 2: Real-Time Messaging Begins

- Messages can now be sent in **both directions** at any time.
- No need to wait for requests — the server can push data instantly!

---

## ⚙️ WebSocket Use Cases

- 💬 Chat applications
- 📈 Stock price tickers
- 🎮 Multiplayer gaming
- 🧠 Collaborative editing
- 📡 Live notifications
- 🕐 Real-time dashboards

---

## 🛠 Example Code (Node.js)

```js
const WebSocket = require('ws');

const server = new WebSocket.Server({ port: 8080 });

server.on('connection', socket => {
  console.log('Client connected');
  
  socket.on('message', msg => {
    console.log(`Received: ${msg}`);
    socket.send(`Echo: ${msg}`);
  });

  socket.on('close', () => {
    console.log('Client disconnected');
  });
});
🚧 Limitations
Not ideal for very short-lived connections

Can be blocked by some proxies/firewalls

Requires fallback for older browsers (or use libraries like Socket.IO)

✅ Alternatives & Protocols
Protocol	When to Use
HTTP	One-time fetches (GET/POST requests)
WebSocket	Persistent, full-duplex communication
SSE (Server Sent Events)	Server → Client only updates
gRPC/WebRTC	Special real-time/peer-to-peer needs
💡 Final Thoughts
WebSockets changed the game by enabling true real-time web applications. They're lightweight, fast, and perfect for scenarios where latency matters.

If you're building anything interactive, collaborative, or instant — it’s time to speak WebSocket.