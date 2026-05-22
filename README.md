<!-- full-stack-devoloper/
├── 📝 README.md # Roadmap overview
├── 📂 projects/ # Practical implementations
│ ├── counter-app/ # Week 1
│ ├── todo-list-context/ # Week 2
│ ├── weather-dashboard/ # Week 3
│ ├── blog-app/ # Week 4
│ └── social-media-app/ # Week 5
├── 📘 resources/ # Specs & guides
│ ├── scrimba-links.md # Interactive tutorials
│ ├── java-guide-chapter.md # Core theory
│ ├── waqas-plan.md # Advanced React
│ └── coders-charge.md # Deployment tips
└── ⚙️ study-schedule/ # Timetables & checklists -->

### 🔖 Full .md Roadmap (Copy‑Paste This Into README.md)

# React JS Study Plan

## 📅 Weekly Overview

| Week  | Focus                 | Primary Project                         | Key Concepts                                |
| ----- | --------------------- | --------------------------------------- | ------------------------------------------- |
| **1** | Foundations           | `counter-app/`                          | Components, `useState`, Props               |
| **2** | Core Concepts         | `todo-list-context/` & `shopping-cart/` | Context API, `useReducer`, State Management |
| **3** | APIs & Error Handling | `weather-dashboard/`                    | Fetch, Loading/Error states                 |
| **4** | Full‑Stack App        | `blog-app/`                             | React Router, CRUD, Auth Mock               |
| **5** | Real‑World App        | `social-media-app/`                     | File upload, Firebase, Real‑time updates    |

---

## 📂 Project Directory Layout

projects/
├── counter-app/
│ └── src/Counter.jsx
├── todo-list-context/
│ └── src/TodoProvider.jsx
├── weather-dashboard/
│ └── src/Weather.jsx
├── blog-app/
│ └── src/App.jsx
└── social-media-app/
└── src/App.jsx

---

## 🛠️ Code Templates

### **1️⃣ Counter App (`counter-app/src/Counter.jsx`)**

```jsx
import { useState, useEffect } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Counter: ${count}`;
  }, [count]);

  return (
    <div style={{ textAlign: "center", marginTop: "5rem" }}>
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(prev => prev - 1)}>-</button>
      <button onClick={() => setCount(prev => prev + 1)}>+</button>
      <button
        onClick={() => setCount(0)}
        style={{ marginLeft: "1rem", background: "#f55" }}
      >
        Reset
      </button>
    </div>
  );
}

### 2️⃣ Todo List with Context (todo-list-context/src/TodoProvider.jsx)

import { createContext, useContext, useState, useReducer } from "react";

const TodoContext = createContext();

export function TodoProvider({ children }) {
  const [todos, dispatch] = useReducer((state, action) => {
    switch (action.type) {
      case "ADD":
        return [...state, { ...action.payload, id: Date.now() }];
      case "DELETE":
        return state.filter(t => t.id !== action.payload);
      default:
        return state;
    }
  }, []);

  return (
    <TodoContext.Provider value={{ todos, dispatch }}>
      {children}
    </TodoContext.Provider>
  );
}

export function useTodos() {
  return useContext(TodoContext);
}

### 3️⃣ Weather Dashboard (weather-dashboard/src/Weather.jsx)

import { useEffect, useState } from "react";

export default function Weather({ city = "London" }) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchWeather = async () => {
      try {
        const res = await fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=YOUR_KEY`
        );
        if (!res.ok) throw new Error("Network response was not ok");
        const json = await res.json();
        setData(json);
      } catch (e) {
        setError(e.message);
      } finally {
        setLoading(false);
      }
    };
    fetchWeather();
  }, [city]);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h2>{data.name}</h2>
      <p>{data.weather[0].description}</p>
      <p>Temp: {data.main.temp}°C</p>
    </div>
  );
}

### 4️⃣ Blog App (blog-app/src/App.jsx)

import { BrowserRouter as Router, Routes, Route, Link } from "react-router-dom";

function Home() {
  return (
    <div>
      <h1>My Blog</h1>
      <nav>
        <Link to="/posts">Posts</Link> |{" "}
        <Link to="/about">About</Link>
      </nav>
    </div>
  );
}

function Posts() {
  // Mock posts data
  const posts = [
    { id: 1, title: "First Post", content: "Hello World!" },
    // ... more
  ];

  return (
    <div>
      {posts.map(p => (
        <article key={p.id}>
          <h2>{p.title}</h2>
          <p>{p.content}</p>
        </article>
      ))}
    </div>
  );
}

function About() {
  return <div><h1>About Us</h1></div>;
}

export default function App() {
  return (
    <Router>
      <div>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/posts" element={<Posts />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </div>
    </Router>
  );
}

### 5️⃣ Social Media App (social-media-app/src/App.jsx)

import { useState } from "react";
import { useDropzone } from "react-dropzone";

export default function SocialApp() {
  const [files, setFiles] = useState([]);
  const [posts, setPosts] = useState([]);

  const onDrop = (acceptedFiles) => {
    setFiles(prev => [...prev, ...acceptedFiles]);
  };

  const { getRootProps, listenDrop, getContainerProps } = useDropzone({
    onDrop,
    accept: { "image/*": [".png", ".jpg", ".jpeg", ".gif"] },
  });

  const addPost = (file) => {
    setPosts(prev => [...prev, { id: Date.now(), image: URL.createObjectURL(file) }]);
  };

  return (
    <div>
      <div {...getContainerProps()} {...getRootProps()} />
      {files.map(addPost)}
      <div style={{ marginTop: "2rem" }}>
        {posts.map(p => (
          <div key={p.id} style={{ marginBottom: "1rem" }}>
            <img src={p.image} alt="post" style={{ maxWidth: "300px" }} />
            <p>New post!</p>
          </div>
        ))}
      </div>
    </div>
  );
}

———

## 🚀 GitHub Repository Setup (CLI)

# 1️⃣ Initialize Git
git init

# 2️⃣ Add all files
git add .

# 3️⃣ Commit
git commit -m "Initial commit – React study plan scaffold"

# 4️⃣ Create a remote repo on GitHub (via GitHub CLI)
# Install GitHub CLI first: https://cli.github.com/
gh auth login   # authenticate
gh repo create full-stack-devoloper --public --source=. --remote=origin

# 5️⃣ Push
git push -u origin main

Alternative (Web UI)

1. Go to https://github.com/new
2. Name the repo full-stack-devoloper
3. Upload the folder contents or push via SSH

———

### Next Steps

- Day 1: Clone the repo, install Node ≥ 20, run npm create vite@latest counter-app -- --template react.
- Day 2‑5: Follow the weekly project checklist above.
- Weekly Review: Open a Pull Request for each completed project, request feedback, and merge.

———

💡 Tips

- Use Branches (feature/counter, feature/todo-context) for isolated work.
- Run npm run test (Jest + React Testing Library) after each major change.
- Deploy each project to Vercel (vercel --prod) for live preview.

———

You now have:

1. ✅ A ready‑to‑copy README.md with the full roadmap.
2. ✅ Code templates for every weekly project.
3. ✅ Step‑by‑step instructions to spin up a GitHub repo and start version‑controlling your work.

Happy coding! 🚀
```
