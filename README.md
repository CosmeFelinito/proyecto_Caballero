# proyecto_Caballero
git init
git remote add origin https://github.com/TU-USUARIO/todo-app.git
node_modules/
dist/
.env
git add .
git commit -m "Commit inicial: backend con Express y frontend con React"
git branch -M main
git push -u origin main
todo-app/

---

## ⚙️ Instalación y ejecución

### 1. Clonar el repositorio
```bash
git clone https://github.com/TU-USUARIO/todo-app.git
cd todo-app
cd backend
npm install
npm start
cd ../frontend
npm install
npm run dev
PORT=3000
VITE_API_URL=http://localhost:3000/api

---
