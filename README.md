# first-epam-ceo
Student Management System
project structure 
smart-task-manager/
├── index.html
├── style.css
├── script.js
└── README.md
html code

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Task Manager</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="container">
        <header>
            <h1>🚀 Smart Task Manager</h1>
            <button id="themeBtn" onclick="toggleTheme()">🌙</button>
        </header>

        <p class="subtitle">Organize your day, achieve your goals.</p>

        <div class="task-input">
            <input type="text" id="taskInput"
                placeholder="Enter your task...">

            <select id="priority">
                <option value="Low">Low</option>
                <option value="Medium">Medium</option>
                <option value="High">High</option>
            </select>

            <button onclick="addTask()">Add Task +</button>
        </div>

        <input type="text" id="searchInput"
            placeholder="🔍 Search tasks..." oninput="renderTasks()">

        <div class="stats">
            <div>Total: <span id="total">0</span></div>
            <div>Completed: <span id="completed">0</span></div>
            <div>Pending: <span id="pending">0</span></div>
        </div>

        <div class="filters">
            <button onclick="setFilter('All')">All</button>
            <button onclick="setFilter('Pending')">Pending</button>
            <button onclick="setFilter('Completed')">Completed</button>
        </div>

        <ul id="taskList"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>
#css code

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    background: linear-gradient(135deg, #667eea, #764ba2);
    min-height: 100vh;
    padding: 40px 15px;
}

.container {
    max-width: 750px;
    margin: auto;
    background: white;
    padding: 30px;
    border-radius: 18px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

h1 {
    color: #333;
    font-size: 27px;
}

#themeBtn {
    border: none;
    background: #eee;
    padding: 10px;
    border-radius: 8px;
    cursor: pointer;
}

.subtitle {
    color: #777;
    margin: 12px 0 25px;
}

.task-input {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

input, select {
    padding: 13px;
    border: 1px solid #ddd;
    border-radius: 8px;
    outline: none;
    min-width: 0;
}

#taskInput {
    flex: 1;
}

button {
    cursor: pointer;
    border: none;
    border-radius: 8px;
    padding: 12px 16px;
    background: #667eea;
    color: white;
    transition: 0.2s;
}

button:hover {
    opacity: 0.85;
}

#searchInput {
    width: 100%;
    margin-bottom: 20px;
}

.stats {
    display: flex;
    justify-content: space-around;
    background: #f3f4ff;
    padding: 18px;
    border-radius: 10px;
    margin-bottom: 20px;
    gap: 10px;
}

.filters {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

#taskList {
    list-style: none;
}

.task {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    padding: 15px;
    border: 1px solid #eee;
    border-radius: 10px;
    margin-bottom: 12px;
}

.task-info {
    flex: 1;
    overflow-wrap: anywhere;
}

.task-info small {
    display: block;
    margin-top: 6px;
    color: #777;
}

.completed {
    text-decoration: line-through;
    color: #999;
}

.delete {
    background: #ef4444;
}

.priority-High {
    color: #ef4444 !important;
}

.priority-Medium {
    color: #e59b17 !important;
}

.priority-Low {
    color: #16a34a !important;
}

body.dark {
    background: #151525;
}

.dark .container {
    background: #25253a;
    color: white;
}

.dark h1 {
    color: white;
}

.dark .subtitle {
    color: #bbb;
}

.dark .stats {
    background: #353550;
}

.dark input, .dark select {
    background: #353550;
    color: white;
    border-color: #555;
}

.dark .task {
    border-color: #45455a;
}

@media(max-width: 600px) {
    .task-input {
        flex-wrap: wrap;
    }

    #taskInput {
        width: 100%;
        flex-basis: 100%;
    }

    .stats {
        flex-direction: column;
    }

    h1 {
        font-size: 21px;
    }
}
#script.java

let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
let currentFilter = "All";

function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function addTask() {
    const input = document.getElementById("taskInput");
    const priority = document.getElementById("priority");

    const text = input.value.trim();

    if (!text) {
        alert("Please enter a task!");
        return;
    }

    tasks.push({
        id: Date.now(),
        text: text,
        priority: priority.value,
        completed: false
    });

    input.value = "";

    saveTasks();
    renderTasks();
}

function deleteTask(id) {
    tasks = tasks.filter(task => task.id !== id);
    saveTasks();
    renderTasks();
}

function toggleTask(id) {
    tasks = tasks.map(task => {
        if (task.id === id) {
            task.completed = !task.completed;
        }
        return task;
    });

    saveTasks();
    renderTasks();
}

function setFilter(filter) {
    currentFilter = filter;
    renderTasks();
}

function renderTasks() {
    const list = document.getElementById("taskList");
    const search = document.getElementById("searchInput")
        .value.toLowerCase();

    list.innerHTML = "";

    const filteredTasks = tasks.filter(task => {
        const matchesSearch = task.text.toLowerCase().includes(search);

        const matchesFilter =
            currentFilter === "All" ||
            (currentFilter === "Pending" && !task.completed) ||
            (currentFilter === "Completed" && task.completed);

        return matchesSearch && matchesFilter;
    });

    filteredTasks.forEach(task => {
        const li = document.createElement("li");
        li.className = "task";

        const info = document.createElement("div");
        info.className = "task-info";

        const title = document.createElement("span");
        title.textContent = task.text;

        if (task.completed) {
            title.classList.add("completed");
        }

        const priority = document.createElement("small");
        priority.className = `priority-${task.priority}`;
        priority.textContent = `Priority: ${task.priority}`;

        info.append(title, priority);

        const completeBtn = document.createElement("button");
        completeBtn.textContent = task.completed ? "Undo" : "Done";
        completeBtn.onclick = () => toggleTask(task.id);

        const deleteBtn = document.createElement("button");
        deleteBtn.textContent = "Delete";
        deleteBtn.className = "delete";
        deleteBtn.onclick = () => deleteTask(task.id);

        li.append(info, completeBtn, deleteBtn);
        list.appendChild(li);
    });

    document.getElementById("total").textContent = tasks.length;

    const completedCount =
        tasks.filter(task => task.completed).length;

    document.getElementById("completed").textContent =
        completedCount;

    document.getElementById("pending").textContent =
        tasks.length - completedCount;
}

function toggleTheme() {
    document.body.classList.toggle("dark");

    const dark = document.body.classList.contains("dark");

    document.getElementById("themeBtn").textContent =
        dark ? "☀️" : "🌙";

    localStorage.setItem("darkMode", dark);
}

if (localStorage.getItem("darkMode") === "true") {
    document.body.classList.add("dark");
    document.getElementById("themeBtn").textContent = "☀️";
}

renderTasks();
