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
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import TaskList from './components/TaskList';
import TaskForm from './components/TaskForm';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<TaskList />} />
        <Route path="/new" element={<TaskForm />} />
        <Route path="/edit/:id" element={<TaskForm />} />
      </Routes>
    </Router>
  );
}

export default App;
import { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';

function TaskList() {
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    fetch(`${import.meta.env.VITE_API_URL}/tasks`)
      .then(res => res.json())
      .then(data => setTasks(data));
  }, []);

  return (
    <div>
      <h1>Lista de Tareas</h1>
      <Link to="/new">➕ Nueva tarea</Link>
      <ul>
        {tasks.map(task => (
          <li key={task.id}>
            <Link to={`/edit/${task.id}`}>{task.title}</Link> - {task.completed ? "✅" : "❌"}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TaskList;
import { useState, useEffect } from 'react';
import { useNavigate, useParams } from 'react-router-dom';

function TaskForm() {
  const { id } = useParams();
  const navigate = useNavigate();
  const [title, setTitle] = useState('');
  const [completed, setCompleted] = useState(false);

  useEffect(() => {
    if (id) {
      fetch(`${import.meta.env.VITE_API_URL}/tasks`)
        .then(res => res.json())
        .then(data => {
          const task = data.find(t => t.id == id);
          if (task) {
            setTitle(task.title);
            setCompleted(task.completed);
          }
        });
    }
  }, [id]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const method = id ? 'PUT' : 'POST';
    const url = id
      ? `${import.meta.env.VITE_API_URL}/tasks/${id}`
      : `${import.meta.env.VITE_API_URL}/tasks`;

    await fetch(url, {
      method,
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title, completed })
    });

    navigate('/');
  };

  return (
    <form onSubmit={handleSubmit}>
      <h1>{id ? 'Editar Tarea' : 'Nueva Tarea'}</h1>
      <input
        type="text"
        placeholder="Título"
        value={title}
        onChange={e => setTitle(e.target.value)}
      />
      <label>
        Completada:
        <input
          type="checkbox"
          checked={completed}
          onChange={e => setCompleted(e.target.checked)}
        />
      </label>
      <button type="submit">Guardar</button>
    </form>
  );
}

export default TaskForm;
body {
  font-family: Arial, sans-serif;
  margin: 20px;
  background: #f4f4f4;
}

h1 {
  color: #333;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: #fff;
  margin: 5px 0;
  padding: 10px;
  border-radius: 5px;
}
