# Cloud Architecture Overview

This monorepo runs as a client-server TODO application. The React frontend serves the user
interface and sends task requests to the Express API. The API reads and writes tasks in an
in-memory SQLite database, so stored data is lost whenever the backend process stops.

```mermaid
C4Context
  title TODO App System Context

  Person(user, "TODO App User", "Creates, updates, completes, and deletes tasks.")

  System_Boundary(todo_app, "TODO App Monorepo") {
    System(frontend, "React Frontend", "Browser UI built with React and Material UI.")
    System(api, "Express API", "Node.js REST API for task operations.")
    SystemDb(store, "In-Memory SQLite Store", "Process-local task data; cleared when the API stops.")
  }

  Rel(user, frontend, "Uses", "Web browser")
  Rel(frontend, api, "Calls /api/tasks", "HTTP/JSON")
  Rel(api, store, "Reads and writes tasks", "better-sqlite3")

  UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

## Create a TODO Sequence

```mermaid
sequenceDiagram
  actor User
  participant Form as React TaskForm
  participant App as React App
  participant API as Express API
  participant Store as In-Memory SQLite

  User->>Form: Enter task details and submit
  Form->>App: onSave(task)
  App->>API: POST /api/tasks (JSON)
  API->>API: Validate required title
  API->>Store: INSERT task
  Store-->>API: New task ID
  API->>Store: SELECT created task
  Store-->>API: Created task
  API-->>App: 201 Created (task JSON)
  App->>API: GET /api/tasks
  API->>Store: SELECT tasks
  Store-->>API: Task rows
  API-->>App: 200 OK (task list)
  App-->>User: Display updated TODO list
```

## Runtime Notes

- The frontend development server proxies `/api` requests to the backend at
  `http://localhost:3030`.
- The root workspace starts the frontend and backend together with `npm start`.
- The store is not a managed cloud database and provides no persistence across backend
  restarts.
