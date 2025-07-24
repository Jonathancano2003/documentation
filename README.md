# Documentación Express.js, Socket.IO y Knex.js

Este repositorio contiene tres proyectos desarrollados con Node.js, Express, WebSocket, Knex y Quasar. A continuación se explica cómo funcionan, cómo usarlos y para qué sirve cada uno.

---

## Índice

- [Express.js](#expressjs)
- [Socket.IO](#socketio)
- [Knex.js](#knexjs)
- [Proyecto 1 – serverjs](#-proyecto-1--serverjs)
- [Proyecto 2 – MI-APP (CRUD + Knex)](#-proyecto-2--mi-app-crud--knex)
- [Proyecto 3 – websocket3 (WebSocket + Quasar)](#-proyecto-3--websocket3-websocket--quasar)


---

## Express.js

### Crear servidor:
```js
const express = require('express');
const app = express();
```

### Definir puerto y arrancar el servidor:
```js
app.listen(3000, () => {
  console.log('Servidor en http://localhost:3000');
});
```

### Leer datos JSON del body:
```js
app.use(express.json());
```

### Ruta GET:
```js
app.get('/hola', (req, res) => {
  res.send('Hola mundo');
});
```

### Ruta POST:
```js
app.post('/usuario', (req, res) => {
  res.json({ datosRecibidos: req.body });
});
```

### Enviar JSON:
```js
res.json({ mensaje: 'Todo OK' });
```

### Texto plano:
```js
res.send('Texto simple');
```

### Ruta con parámetro:
```js
app.get('/user/:id', (req, res) => {
  res.send(`ID recibido: ${req.params.id}`);
});
```

---

## Socket.IO

### Crear servidor WebSocket:
```js
const express = require('express');
const http = require('http');
const { Server } = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = new Server(server, { cors: { origin: '*' } });

server.listen(3000, () => {
  console.log('Servidor WebSocket en http://localhost:3000');
});
```

### Eventos:
```js
io.on('connection', (socket) => {
  console.log('Cliente conectado');

  socket.on('message', (msg) => {
    console.log('Mensaje recibido:', msg);
  });

  socket.broadcast.emit('message', { texto: 'Solo a los demás' });
  socket.emit('message', { texto: 'Solo tú ves esto' });

  socket.on('disconnect', () => {
    console.log('Cliente desconectado');
  });
});
```

### Cliente (frontend):
```js
import { io } from 'socket.io-client';
const socket = io('http://localhost:3000');

socket.emit('message', { texto: 'Hola servidor' });
socket.on('message', (data) => {
  console.log('Mensaje del servidor:', data);
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
db('users').where({ id: 1 }).update({ username: 'nuevoNombre' });
db('users').where({ id: 1 }).del();
```

### Orden y límite:
```js
db('users').orderBy('id', 'desc');
db('users').limit(5);
db('users').where('username', 'like', '%juan%').select('id', 'username').limit(3);
```

---

## 📁 Proyecto 1 – serverjs

### 📄 Descripción
Servidor Express minimalista para probar rutas con Postman y entender cómo funcionan los middlewares.

### 🧱 Tecnologías
- Express.js

### ⚙️ Funcionalidad
- Middleware para JSON, texto, raw, urlencoded
- Rutas GET y POST básicas
- Uso de parámetros en la URL y body

### 📦 Instalación
```bash
cd serverjs
npm install
node index.js
```

Servidor en: `http://localhost:3000`

---

## 📁 Proyecto 2 – MI-APP (CRUD + Knex)

### 📄 Descripción
CRUD de usuarios con autenticación simple. Usa Express y Knex con conexión a base de datos.

### 🧱 Tecnologías
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

### 📦 Instalación
```bash
cd backend
npm install
node index.js
```

> Asegúrate de tener la tabla `users` creada.

---

## 📁 Proyecto 3 – websocket3 (WebSocket + Quasar)

### 📄 Descripción
App de frontend en Quasar que recibe datos por WebSocket y también consulta una ruta HTTP.

### 🧱 Tecnologías
- Express.js
- Socket.IO
- Quasar (Vue 3)

### ⚙️ Funcionalidad
**Backend**:
- WebSocket emite valores periódicos (`update1`, `update2`, `update3`)
- `GET /api/mensaje` responde por HTTP
- Recibe mensajes desde el frontend

**Frontend**:
- Muestra valores en tiempo real
- Envío de mensajes vía WebSocket
- Llamada a HTTP y cambio de página

### 📦 Instalación
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

> Backend debe estar corriendo en puerto `3001`.

---

## ✅ Requisitos generales

- Node.js
- `npm install` en cada proyecto
- Tener base de datos (en MI-APP)
- `npm install -g @quasar/cli` para usar Quasar

---

## ✍️ Autor

Jonathan Cano
