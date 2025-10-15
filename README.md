## Task Manager App Using React
# Name : Amirthavarshini.R.D
# Reg.No: 212223040013              
Task Manager App

Problem:
Set 1 — Task Manager App
Problem:

Build a Task Manager application where users can:

Add tasks

Mark tasks as done

Delete tasks

Filter by completed/uncompleted tasks

You must use:

useReducer → to manage the list of tasks (ADD, TOGGLE, DELETE)

useRef → to focus the input box after each task is added

useMemo → to memoize the filtered task list (so filtering is fast even with many tasks)

useCallback → to memoize event handlers like handleAdd and handleToggle so that child components don’t re-render unnecessarily

Hints:

useReducer manages the tasks state

useMemo filters tasks based on filterType

useCallback wraps add/delete/toggle handlers

useRef focuses the task input field

Code for TaskManager App:

```
// src/TaskManager.jsx
import { useReducer, useRef, useMemo, useCallback, useState } from "react";

const initialTasks = [];

function taskReducer(state, action) {
  switch (action.type) {
    case "ADD":
      return [...state, { id: Date.now(), text: action.payload, completed: false }];
    case "TOGGLE":
      return state.map((task) =>
        task.id === action.payload ? { ...task, completed: !task.completed } : task
      );
    case "DELETE":
      return state.filter((task) => task.id !== action.payload);
    default:
      return state;
  }
}

export default function TaskManager() {
  const [tasks, dispatch] = useReducer(taskReducer, initialTasks);
  const [filter, setFilter] = useState("all");
  const inputRef = useRef();

  const handleAdd = useCallback(() => {
    const text = inputRef.current.value.trim();
    if (text) {
      dispatch({ type: "ADD", payload: text });
      inputRef.current.value = "";
      inputRef.current.focus();
    }
  }, []);

  const handleToggle = useCallback((id) => {
    dispatch({ type: "TOGGLE", payload: id });
  }, []);

  const handleDelete = useCallback((id) => {
    dispatch({ type: "DELETE", payload: id });
  }, []);

  const filteredTasks = useMemo(() => {
    if (filter === "completed") return tasks.filter((t) => t.completed);
    if (filter === "active") return tasks.filter((t) => !t.completed);
    return tasks;
  }, [tasks, filter]);

  return (
    <div style={styles.container}>
      <h1 style={styles.heading}>Task Manager</h1>

      <div style={styles.inputArea}>
        <input ref={inputRef} type="text" placeholder="Enter a new task" style={styles.input} />
        <button onClick={handleAdd} style={styles.button}>Add</button>
      </div>

      <div style={styles.filters}>
        <button onClick={() => setFilter("all")}>All</button>
        <button onClick={() => setFilter("active")}>Active</button>
        <button onClick={() => setFilter("completed")}>Completed</button>
      </div>

      <ul style={styles.taskList}>
        {filteredTasks.map((task) => (
          <li key={task.id} style={styles.taskItem}>
            <input
              type="checkbox"
              checked={task.completed}
              onChange={() => handleToggle(task.id)}
            />
            <span
              style={{
                textDecoration: task.completed ? "line-through" : "none",
                marginLeft: 8,
              }}
            >
              {task.text}
            </span>
            <button onClick={() => handleDelete(task.id)} style={styles.deleteButton}>
              ❌
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

const styles = {
  container: {
    maxWidth: 400,
    margin: "50px auto",
    padding: 20,
    borderRadius: 10,
    boxShadow: "0 0 10px rgba(0,0,0,0.2)",
    backgroundColor: "#fff",
    textAlign: "center",
    fontFamily: "Arial, sans-serif",
  },
  heading: { marginBottom: 20 },
  inputArea: { display: "flex", justifyContent: "center", gap: 8 },
  input: { flex: 1, padding: 8, fontSize: 16 },
  button: { padding: "8px 12px", cursor: "pointer" },
  filters: { marginTop: 15, display: "flex", justifyContent: "center", gap: 8 },
  taskList: { listStyle: "none", padding: 0, marginTop: 20 },
  taskItem: {
    display: "flex",
    alignItems: "center",
    justifyContent: "space-between",
    backgroundColor: "#f9f9f9",
    padding: "8px 12px",
    marginBottom: 8,
    borderRadius: 6,
  },
  deleteButton: { background: "none", border: "none", cursor: "pointer", fontSize: 16 },
};


```
App.jsx


```
// src/App.jsx
import TaskManager from "./TaskManager";

function App() {
  return (
    <div>
      <TaskManager />
    </div>
  );
}

export default App;


```
## Output :
<img width="1874" height="977" alt="image" src="https://github.com/user-attachments/assets/57344fbd-c6fd-435f-8025-d054352c94a6" />
