# Proyecto CRUD de Usuarios (Fullstack Sencillo)

Este proyecto cuenta con una arquitectura de 2 carpetas principales: `backend` y `frontend`.

## 🛠️ Tecnologías Utilizadas
##actu  mhjghjgh

- **Backend**: Node.js, Fastify, Prisma ORM, PostgreSQL, TypeScript.
- **Frontend**: Vite, React, Mantine UI v7, Tabler Icons, TypeScript.

---
#carga automática en jenkins
# carga kubenetes prueba
## 📂 Estructura del Proyecto

```
practica_2_maestria/
├── backend/
│   ├── prisma/
│   │   └── schema.prisma      # Modelo User para PostgreSQL
│   ├── src/
│   │   ├── lib/
│   │   │   └── prisma.ts      # Cliente de Prisma singleton
│   │   ├── routes/
│   │   │   └── userRoutes.ts  # Endpoints CRUD (GET, POST, PUT, DELETE)
│   │   └── server.ts          # Servidor Fastify con CORS
│   ├── .env                   # Cadena de conexión DATABASE_URL
│   ├── package.json
│   └── tsconfig.json
│
└── frontend/
    ├── src/
    │   ├── components/
    │   │   └── UserCrud.tsx   # Interfaz interactiva con Mantine UI (Tabla + Modales)
    │   ├── App.tsx            # Proveedor de Mantine y Layout
    │   ├── main.tsx
    │   └── index.css          # Estilos visuales con tema oscuro y glassmorphism
    ├── index.html
    ├── package.json
    ├── postcss.config.cjs
    └── vite.config.ts
```

---

##  Pasos para Ejecutar el Proyecto

### 1. Configurar Base de Datos PostgreSQL
1. Asegúrate de tener PostgreSQL ejecutándose localmente (o en Docker).
2. Modifica la variable `DATABASE_URL` en `backend/.env` con tus credenciales:
   ```env
   DATABASE_URL="postgresql://usuario:contraseña@localhost:5432/nombre_bd?schema=public"
   ```

### 2. Iniciar el Backend
```bash
cd backend
npm install

# Crear las tablas en la base de datos PostgreSQL mediante Prisma
npx prisma db push

# Iniciar servidor en modo desarrollo (Puerto 3001)
npm run dev
```

### 3. Iniciar el Frontend
En otra terminal:
```bash
cd frontend
npm install

# Iniciar aplicación React con Vite (Puerto 5173)
npm run dev
```

Abre tu navegador en `http://localhost:5173` para interactuar con la aplicación.

# Prueba webhook Jenkins
Actualizacion menor para validar disparo automatico desde GitHub hacia Jenkins.
Prueba de activacion automatica de Jenkins por webhook.
Segunda Prueba de activacion automatica de Jenkins por webhook.