# Saline

A robust, high-performance, type-safe full-stack template combining a **Rust** backend (powered by **Salvo** and **SeaORM**) with a modern **Vue 3 + Vite** frontend. This architecture can be deployed as a single, lightweight standalone binary.

---

## 🏗️ High-Level Architecture

* **Frontend (Vue 3 + Vite):** Runs in the browser, communicating with the backend via REST APIs. During development, Vite proxies API requests to the Salvo server to bypass CORS.
* **Backend (Salvo):** A high-performance HTTP server built on Tokio and Hyper, managing tree-based routing, authentication middleware, and request validation.
* **Database Layer (SeaORM):** An async, dynamic ORM for Rust interfacing with your database (PostgreSQL, MySQL, or SQLite) with type-safe query building.

---

## 📁 Project Directory Structure

```text
my-fullstack-app/
├── Cargo.toml             # Workspace or root Cargo config
├── backend/               # Rust Backend (Salvo + SeaORM)
│   ├── Cargo.toml
│   └── src/
│       ├── entities/      # SeaORM generated entities
│       ├── handlers/      # Salvo request handlers
│       ├── routes/        # Salvo tree routing setup
│       └── main.rs        # Server entry point
└── frontend/              # Vue 3 Frontend (Vite)
    ├── package.json
    ├── vite.config.ts     # Configure proxy to Salvo backend
    └── src/

```

---

## 🚀 Getting Started

### Prerequisites

Ensure you have the following installed on your system:

* [Rust & Cargo](https://www.rust-lang.org/tools/install) (latest stable)
* [Node.js & npm](https://nodejs.org/) (v18+ recommended)

---

### Step 1: Backend Setup (Salvo + SeaORM)

1. Navigate to the backend directory:
```bash
cd backend

```


2. Run the Salvo server (it will automatically connect to SQLite and set up local storage if configured):
```bash
cargo run

```


The backend server will start at `[http://127.0.0.1:5800](http://127.0.0.1:5800)`.

---

### Step 2: Frontend Setup (Vue 3 + Vite)

1. Open a new terminal and navigate to the frontend directory:
```bash
cd frontend

```


2. Install dependencies:
```bash
npm install

```


3. Start the Vite development server:
```bash
npm run dev

```


The frontend will run at `http://localhost:5173`. Vite is pre-configured to proxy `/api` requests to your Salvo backend on port `5800`.

---

## 📦 Production Deployment (Single Binary)

One of Salvo's primary strengths is serving static assets out-of-the-box. You can bundle your entire full-stack app into a single executable:

1. Build the Vue frontend production assets:
```bash
cd frontend && npm run build

```


2. Serve the generated `dist` folder inside your Salvo router (`backend/src/main.rs`):
```rust
// Serve static files from the Vue build directory
let router = Router::new()
    .push(Router::with_path("assets/<**path>").get(StaticDir::new(["frontend/dist/assets"])))
    .push(Router::with_path("<*path>").get(NamedFile::new("frontend/dist/index.html")));

```


3. Build and run the release binary:
```bash
cargo build --release

```
