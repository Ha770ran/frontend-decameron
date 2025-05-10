# 🖥️ Frontend - Decameron Hoteles

Este proyecto es la interfaz web del sistema de gestión de hoteles y habitaciones de Decameron. Permite listar hoteles, registrar nuevos, editar información, gestionar habitaciones por hotel, y visualizar todo en una UI moderna y responsiva.

Desarrollado con **Vue 3 + Vite**, desplegado en **Vercel** y conectado al backend en **Render**.

---

## 🚀 Tecnologías Utilizadas

| Componente    | Tecnología          |
|---------------|----------------------|
| Framework     | Vue 3                |
| Empaquetador  | Vite                 |
| Comunicación  | Axios (HTTP)         |
| Estilo        | CSS / Flexbox        |
| Despliegue    | Vercel               |

---

## 📁 Estructura del Proyecto

frontend-decameron/
├── src/
│ ├── components/
│ │ ├── HotelForm.vue
│ │ ├── HotelList.vue
│ │ ├── RoomForm.vue
│ │ └── RoomList.vue
│ ├── App.vue
│ └── main.js
├── public/
├── .env
├── index.html
└── README.md


---

## ⚙️ Instalación local

### 🔸 Requisitos

- Node.js (recomendado: 18.x)
- npm (o yarn)
- Acceso al backend ya desplegado (Render)

---

### 🔸 Pasos

1. Clona el repositorio:

```bash
git clone https://github.com/Ha770ran/frontend-decameron.git
cd frontend-decameron

## 2. Instala las dependencias:

bash:
npm install

### 3. Crea un archivo .env en la raíz:

VITE_API_URL=https://decameron-backend.onrender.com


#### 4. Ejecuta el proyecto local:

bash:
npm run dev

🌐 Versión en línea
El proyecto está desplegado en:

👉 https://frontend-decameron.vercel.app

⚙️ Conecta directamente al backend de Render para hacer peticiones REST.

✨ Funcionalidades
Crear hoteles con validaciones HTML

Editar y eliminar hoteles

Ver habitaciones por hotel

Crear y editar habitaciones

UI responsiva (adaptada a móviles y pantallas medianas)

Conexión directa al backend con Axios

Manejo de errores en pantalla

📦 Despliegue en Vercel
Repositorio enlazado a Vercel:
https://vercel.com/dashboard

Variables de entorno configuradas en Vercel:

VITE_API_URL=https://decameron-backend.onrender.com

Script de build detectado automáticamente:

vite build

💡 Notas adicionales
Este proyecto es parte de una solución full stack PHP + PostgreSQL + Vue.
Está listo para ampliarse con login, filtros o dashboard administrativo.

👤 Autor
Nombre: Haloran Barney Iglesias
GitHub: @Ha770ran
Email: hebi.tech@gmail.com



