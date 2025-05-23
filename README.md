# project2004
// Basic Full-Stack DevConnector App - React Frontend + Express Backend (Simplified Version)

// This is a simplified example of a fullstack app in a single file format for GitHub demo purposes only.

/* FILE STRUCTURE (recommended)
root
|-- client (React frontend)
|-- server (Node.js backend)
*/

/* ---- server/index.js ---- */
const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');
const app = express();
const PORT = 5000;

app.use(cors());
app.use(express.json());

mongoose.connect('mongodb://localhost:27017/devconnector', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const UserSchema = new mongoose.Schema({
  username: String,
  bio: String,
});

const User = mongoose.model('User', UserSchema);

app.get('/api/users', async (req, res) => {
  const users = await User.find();
  res.json(users);
});

app.post('/api/users', async (req, res) => {
  const newUser = new User(req.body);
  await newUser.save();
  res.json(newUser);
});

app.listen(PORT, () => console.log(`Server running on http://localhost:${PORT}`));

/* ---- client/src/App.jsx ---- */
import React, { useState, useEffect } from 'react';
import axios from 'axios';

export default function App() {
  const [users, setUsers] = useState([]);
  const [formData, setFormData] = useState({ username: '', bio: '' });

  useEffect(() => {
    axios.get('http://localhost:5000/api/users').then(res => setUsers(res.data));
  }, []);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const res = await axios.post('http://localhost:5000/api/users', formData);
    setUsers([...users, res.data]);
    setFormData({ username: '', bio: '' });
  };

  return (
    <div className="p-4 max-w-xl mx-auto">
      <h1 className="text-2xl font-bold mb-4">DevConnector</h1>
      <form onSubmit={handleSubmit} className="mb-6">
        <input className="border p-2 w-full mb-2" placeholder="Username" value={formData.username} onChange={e => setFormData({ ...formData, username: e.target.value })} />
        <textarea className="border p-2 w-full mb-2" placeholder="Bio" value={formData.bio} onChange={e => setFormData({ ...formData, bio: e.target.value })} />
        <button type="submit" className="bg-blue-500 text-white px-4 py-2 rounded">Add User</button>
      </form>
      <div>
        {users.map((user, idx) => (
          <div key={idx} className="border-b py-2">
            <p className="font-semibold">{user.username}</p>
            <p>{user.bio}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

/* ---- client/src/main.jsx ---- */
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')).render(<App />);

/* ---- client/index.html ---- */
<!-- Add Tailwind CDN for quick setup -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>DevConnector</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>

/* ---- .env (server) ---- */
// MONGO_URI=mongodb://localhost:27017/devconnector

/* ---- README.md ---- */
# DevConnector (Mini Fullstack App)

### Setup
1. `cd server` → `npm install` → `node index.js`
2. `cd client` → `npm install` → `npm run dev`

This is a basic fullstack app using React and Express/MongoDB.
