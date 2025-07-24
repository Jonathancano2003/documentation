# Express.js, Socket.IO and Knex.js Documentation

This repository contains three projects developed using Node.js, Express, WebSocket, Knex, and Quasar. Below is an explanation of how they work, how to use them, and their purpose.

---

## Index

- [Express.js](#expressjs)
- [Socket.IO](#socketio)
- [Knex.js](#knexjs)
- [Project 1 – serverjs](#project-1--serverjs)
- [Project 2 – MI-APP (CRUD + Knex)](#project-2--mi-app-crud--knex)
- [Project 3 – websocket3 (WebSocket + Quasar)](#project-3--websocket3-websocket--quasar)

---

## Express.js

### Create server:
```js
const express = require('express');
const app = express();
```

### Define port and start the server:
```js
app.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

### Parse JSON body:
```js
app.use(express.json());
```

### GET route:
```js
app.get('/hola', (req, res) => {
  res.send('Hello world');
});
```

### POST route:
```js
app.post('/usuario', (req, res) => {
  res.json({ receivedData: req.body });
});
```

### Send JSON:
```js
res.json({ message: 'All good' });
```

### Plain text:
```js
res.send('Simple text');
```

### Route with parameter:
```js
app.get('/user/:id', (req, res) => {
  res.send(`Received ID: ${req.params.id}`);
});
```

---

## Socket.IO

### Create WebSocket server:
```js
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server, { cors: { origin: '*' } });

server.listen(3000, () => {
  console.log('WebSocket server at http://localhost:3000');
});
```

### Events:
```js
io.on('connection', (socket) => {
  console.log('Client connected');

  socket.on('message', (msg) => {
    console.log('Received message:', msg);
  });

  socket.broadcast.emit('message', { text: 'Visible only to others' });
  socket.emit('message', { text: 'Only you see this' });

  socket.on('disconnect', () => {
    console.log('Client disconnected');
  });
});
```

### Client (frontend):
```js
import { io } from 'socket.io-client';
const socket = io('http://localhost:3000');

socket.emit('message', { text: 'Hello server' });
socket.on('message', (data) => {
  console.log('Message from server:', data);
});
```

---

## Knex.js

### Select / Where / Insert / Update / Delete
```js
db('users').select('*');
db('users').select('id', 'username');
db('users').where({ id: 1 });
db('users').insert({ username: 'juan', password: '1234' });
db('users').where({ id: 1 }).update({ username: 'newName' });
db('users').where({ id: 1 }).del();
```

### Order and limit:
```js
db('users').orderBy('id', 'desc');
db('users').limit(5);
db('users').where('username', 'like', '%juan%').select('id', 'username').limit(3);
```

---

## 📁 Project 1 – serverjs

### 📄 Description
Minimal Express server to test routes with Postman and understand how middlewares work.

### 🧱 Technologies
- Express.js

### ⚙️ Features
- Middleware for JSON, text, raw, urlencoded
- Basic GET and POST routes
- Use of URL and body parameters

### 📦 Installation
```bash
cd serverjs
npm install
node index.js
```

Server at: `http://localhost:3000`

---

## 📁 Project 2 – MI-APP (CRUD + Knex)

### 📄 Description
User CRUD with simple authentication. Uses Express and Knex with a database connection.

### 🧱 Technologies
- Express.js
- Knex.js
- SQLite / MySQL

### ⚙️ Endpoints
- `POST /api/login`
- `POST /api/logout`
- `GET /api/users`
- `POST /api/users`
- `PUT /api/users/:id`
- `DELETE /api/users/:id`

### 📦 Installation
```bash
cd backend
npm install
node index.js
```

> Make sure the `users` table exists.

---

## 📁 Project 3 – websocket3 (WebSocket + Quasar)

### 📄 Description
Frontend app in Quasar that receives data via WebSocket and also makes an HTTP request.

### 🧱 Technologies
- Express.js
- Socket.IO
- Quasar (Vue 3)

### ⚙️ Features
**Backend**:
- WebSocket emits periodic values (`update1`, `update2`, `update3`)
- `GET /api/mensaje` responds via HTTP
- Receives messages from frontend

**Frontend**:
- Displays real-time values
- Sends messages via WebSocket
- HTTP request and page navigation

### 📦 Installation
**Backend**:
```bash
npm install
node index.js
```

**Frontend**:
```bash
cd websocket3
quasar dev
```

> Backend must be running on port `3001`.

---

## ✅ General Requirements

- Node.js
- `npm install` in each project
- Have a database (in MI-APP)
- `npm install -g @quasar/cli` to use Quasar

---

## ✍️ Author

Jonathan Cano

