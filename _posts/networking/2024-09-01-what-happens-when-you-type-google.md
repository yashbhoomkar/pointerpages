---
layout: single
title: "What Happens When You Type www.google.com in Your Browser?"
date: 2024-09-01 09:00:00 +0530
categories:
  - networking
tags:
  - browser
  - dns
  - tcp
  - http
  - networking
  - interview
toc: true
classes: wide
---

## 🧠 Introduction

It’s a question every computer science student or aspiring engineer should be able to answer:

> What *actually* happens when you type `www.google.com` into your browser and press **Enter**?

This blog breaks down the entire process — from keystroke to Google’s response — into understandable steps across networking, operating systems, and browser architecture.

---

## 🧩 Step-by-Step Breakdown

### 1. 🧾 URL Interpretation

The browser parses the URL:
- Scheme: `https`
- Host: `www.google.com`
- Path: `/` (default)

It sees it needs a secure connection over HTTPS and prepares for a network request.

---

### 2. 🔍 DNS Resolution

To connect, the browser needs an **IP address**.

- Checks local **DNS cache**
- If not found, it asks the **OS resolver**
- OS checks:
  - `/etc/hosts`
  - Local DNS cache
  - Configured DNS server (e.g. 8.8.8.8)

➡️ Eventually resolves `www.google.com` to something like `142.250.193.68`

---

### 3. 📶 TCP Handshake (with TLS)

Now the browser opens a connection to that IP on port 443 (HTTPS):

- Initiates a **3-way TCP handshake**:
  - SYN →  
  - SYN-ACK ←  
  - ACK →

- Immediately followed by a **TLS handshake** for encryption:
  - ClientHello, ServerHello
  - Key exchange
  - Certificate verification
  - Encrypted channel established

---

### 4. 🌐 Sending the HTTP Request

The browser sends:
GET / HTTP/1.1 Host: www.google.com User-Agent: Chrome/... Accept: /

yaml
Copy
Edit

Over an encrypted TLS connection.

---

### 5. 🖥️ Server Processes the Request

- Google’s load balancer receives the request
- Routes it to a healthy web server
- The server reads the path, headers, and prepares a response

---

### 6. 📩 HTTP Response is Sent

The server replies with:
HTTP/1.1 200 OK Content-Type: text/html Content-Length: 6000

yaml
Copy
Edit

Followed by the actual **HTML document**.

---

### 7. 🧱 Browser Renders the Page

The browser:
- Parses the HTML
- Downloads CSS, JS, images (multiple requests)
- Constructs **DOM + CSSOM → Render Tree**
- Renders the page pixel-by-pixel

---

## 🔁 Optimization Tricks

- **Caching**: DNS, HTTP, and assets (to avoid redundant requests)
- **Persistent TCP connections** (via keep-alive)
- **CDNs** serve content faster from nearby locations
- **Service Workers** can cache assets offline

---

## 💡 Final Thoughts

This seemingly simple action involves:
- Browser engine logic
- OS-level resolution
- Network stack (TCP/IP, TLS)
- Server-side processing
- DOM rendering

Understanding this process is essential for full-stack, backend, and DevOps engineers.

---

🧪 Bonus Tip: Try running `dig www.google.com` or using browser DevTools to see DNS, network timing, and rendering in action!

Want a deep dive into TCP or TLS next? Drop me a topic!

---