# Assignment 06: Personal Finance Tracker — Full-Stack Web Application

*Build a real-world web app using Claude Code, Next.js 16, FastAPI, uv & Neon PostgreSQL*

---

## 🎯 Objective

Build a **Personal Finance Tracker** — a web application where users can log their income and expenses, categorize them, and see a visual dashboard showing where their money goes. You will use **Claude Code** to build this, but this document will make sure you **understand every piece** of what you're building and why.

---

## 📖 Before We Begin: What Are We Actually Building?

Imagine opening an app on your browser. You see:

- A clean dashboard showing your **total income**, **total expenses**, and **remaining balance**
- A **pie chart** showing how much you spent on Food, Transport, Shopping, etc.
- A list of all your transactions (both money coming in and going out)
- A button to **add new transactions**
- The ability to **edit** or **delete** any transaction

That's it. Simple to use, but behind the scenes, there's a full system working together.

---

## 🏗️ The Big Picture: How Web Applications Work

Before we build anything, let's understand how modern web apps are structured. Every web application you use (Instagram, YouTube, Daraz) has **three main parts**:

```
┌─────────────────────────────────────────────────────────────┐
│                     YOUR BROWSER                            │
│                                                             │
│   What you SEE and CLICK on                                 │
│   (buttons, forms, charts, colors)                          │
│                                                             │
│   This is called the ── FRONTEND                            │
│   We will use: Next.js 16 (React framework)                 │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           │ When you click "Add Transaction",
                           │ data travels from your browser
                           │ to the server...
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                     THE SERVER                              │
│                                                             │
│   The BRAIN of your app                                     │
│   Receives requests, processes them, sends back responses   │
│                                                             │
│   This is called the ── BACKEND                             │
│   We will use: FastAPI (Python framework)                   │
│   Running on: Uvicorn (the engine that keeps it alive)      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           │ The server needs to SAVE and
                           │ RETRIEVE data from somewhere
                           │ permanent...
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                     THE DATABASE                            │
│                                                             │
│   Where all your data LIVES permanently                     │
│   Like a super-organized Excel spreadsheet                  │
│                                                             │
│   This is called the ── DATABASE                            │
│   We will use: Neon PostgreSQL (cloud database)             │
└─────────────────────────────────────────────────────────────┘
```

### Why Three Parts?

You might wonder — why not just put everything in one place? Think of it like a restaurant:

| Restaurant       | Web App  | Our Stack         |
| ---------------- | -------- | ----------------- |
| Dining area      | Frontend | Next.js 16        |
| Kitchen          | Backend  | FastAPI + Uvicorn |
| Pantry / Storage | Database | Neon PostgreSQL   |

The dining area (frontend) is where customers interact. The kitchen (backend) processes orders and prepares food. The pantry (database) stores all the ingredients. Each part has a clear job, and they communicate with each other.

---

## 🧰 Understanding Our Tech Stack

### 1. Next.js 16 (Frontend)

**What is it?** A framework for building websites using React (a JavaScript library). We're using **version 16** — the latest major release (October 2025).

**Why are we using it?** Because it gives us a powerful feature called **Server Actions**, and v16 comes with some exciting improvements.

**What's new in Next.js 16?**

- **Turbopack (stable):** The new default bundler — makes your development server start **5-10x faster**. You don't need to do anything, it just works.
- **React 19.2:** The latest version of React with new features like View Transitions (smooth page animations).
- **React Compiler (stable):** Automatically optimizes your components so they re-render less. Makes your app faster without you doing anything.
- **Improved logging:** Better error messages during development, so debugging is easier.

You don't need to memorize all of this — just know that v16 is faster and better out of the box.

**What are Server Actions?** Normally, when your website needs to talk to a backend, you have to write extra code to make that connection (called API calls). Server Actions let you write a function that runs on the server directly from your frontend code. Think of it like having a direct phone line to the kitchen instead of shouting your order across the restaurant.

```
WITHOUT Server Actions:
┌──────────┐    HTTP Request     ┌──────────┐
│ Frontend │ ──────────────────► │ Backend  │
│ (Button  │ ◄────────────────── │ (API)    │
│  Click)  │    HTTP Response    │          │
└──────────┘                     └──────────┘
You have to manually write code to send/receive data.

WITH Server Actions:
┌──────────────────────────────────┐
│ Next.js                          │
│                                  │
│  Button Click                    │
│      │                           │
│      ▼                           │
│  Server Action Function          │
│  (runs on server automatically)  │
│      │                           │
│      ▼                           │
│  Calls our FastAPI backend       │
└──────────────────────────────────┘
The connection is handled for you. Much simpler.
```

**In our app:** Server Actions will be the "middleman" between what the user sees (buttons, forms) and our FastAPI backend.

### 2. FastAPI (Backend)

**What is it?** A Python framework for building APIs (Application Programming Interfaces).

**What is an API?** An API is like a waiter in a restaurant. You (the frontend) don't go into the kitchen yourself. You tell the waiter (API) what you want, and the waiter brings it back.

In technical terms, an API is a set of **URLs (endpoints)** that accept requests and send back data. For example:

| What You Want         | API Endpoint        | Method |
| --------------------- | ------------------- | ------ |
| Get all transactions  | `/transactions`   | GET    |
| Add a new transaction | `/transactions`   | POST   |
| Update a transaction  | `/transactions/5` | PUT    |
| Delete a transaction  | `/transactions/5` | DELETE |

**Why FastAPI?** Because it's written in **Python** (which you already know!), it's fast, and it automatically generates a beautiful documentation page at `/docs` where you can test your API without writing any frontend code.

**What are GET, POST, PUT, DELETE?** These are **HTTP methods** — they tell the server what kind of action you want to perform:

| Method           | What It Does         | Real-Life Example              |
| ---------------- | -------------------- | ------------------------------ |
| **GET**    | Read/fetch data      | Looking at your bank statement |
| **POST**   | Create new data      | Depositing money               |
| **PUT**    | Update existing data | Correcting a wrong entry       |
| **DELETE** | Remove data          | Cancelling a transaction       |

### 3. Uvicorn (Server Runner)

**What is it?** Uvicorn is the engine that **runs** your FastAPI application.

**Why do we need it?** FastAPI is just code — it doesn't run by itself. Uvicorn is like turning the key in your car's ignition. FastAPI is the car, Uvicorn starts the engine.

```
FastAPI (your code)  +  Uvicorn (the runner)  =  A working server

It's like:
Recipe (FastAPI)  +  Stove (Uvicorn)  =  Cooked Food (Running API)
```

When you run the command `uv run uvicorn main:app --reload`, you're telling:

- `uv run` → Use uv to run this command inside our project's virtual environment
- `uvicorn` → Start the Uvicorn server
- `main` → Look in the file called `main.py`
- `app` → Find the FastAPI application object named `app`
- `--reload` → If I change the code, restart automatically (useful during development)

### 4. uv (Python Project Manager)

**What is it?** `uv` is a modern, ultra-fast Python project manager built by Astral (the same people who made `ruff`). It replaces `pip`, `pip-tools`, `virtualenv`, and even `pyenv` — all in one tool.

**Why are we using it instead of pip?**

| Old Way (pip)                       | New Way (uv)                           |
| ----------------------------------- | -------------------------------------- |
| `python -m venv .venv`            | `uv init` (does it all for you)      |
| `pip install fastapi`             | `uv add fastapi`                     |
| `pip install -r requirements.txt` | `uv sync` (auto from pyproject.toml) |
| `pip freeze > requirements.txt`   | Automatic `uv.lock` file             |
| Slow installation                   | **10-100x faster** installation  |

**What does uv do for us?**

1. **Creates the project** (`uv init`) — sets up `pyproject.toml` (the modern replacement for `requirements.txt`)
2. **Manages dependencies** (`uv add fastapi`) — installs packages AND records them automatically
3. **Creates virtual environments** automatically — no more `python -m venv`
4. **Runs commands** (`uv run`) — runs any command inside the project's virtual environment
5. **Locks dependencies** — creates a `uv.lock` file so everyone gets the exact same package versions

**What is `pyproject.toml`?** It's the modern way to describe a Python project. Instead of having separate `requirements.txt`, `setup.py`, and `setup.cfg` files, everything goes in one file:

```
[project]
name = "finance-tracker-backend"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "fastapi[standard]",
    "sqlalchemy",
    "psycopg2-binary",
    "python-dotenv",
]
```

### 5. Neon PostgreSQL (Database)

**What is it?** Neon is a cloud-hosted PostgreSQL database. PostgreSQL is one of the most popular databases in the world.

**What is a database?** Think of it as a permanent, organized storage system. Unlike Python lists or dictionaries that disappear when your program stops, a database keeps data **forever** (until you delete it).

**What is "cloud-hosted"?** Instead of running the database on your own computer, Neon runs it on their servers (in the cloud). You just connect to it via a URL. This means:

- No installation needed
- Your data is accessible from anywhere
- It's free for small projects

**What does data look like in a database?** It looks like a table (similar to Excel):

```
transactions table:
┌────┬─────────────┬────────┬──────────┬──────────┬─────────────┬────────────┐
│ id │ description │ amount │   type   │ category │    date     │  created   │
├────┼─────────────┼────────┼──────────┼──────────┼─────────────┼────────────┤
│  1 │ Salary      │ 50000  │ income   │ salary   │ 2026-02-01  │ 2026-02-01 │
│  2 │ Groceries   │  2500  │ expense  │ food     │ 2026-02-02  │ 2026-02-02 │
│  3 │ Uber ride   │   350  │ expense  │transport │ 2026-02-03  │ 2026-02-03 │
│  4 │ Freelance   │ 15000  │ income   │ freelance│ 2026-02-05  │ 2026-02-05 │
└────┴─────────────┴────────┴──────────┴──────────┴─────────────┴────────────┘
```

Each **row** is one transaction. Each **column** is a piece of information about that transaction.

---

## 🔄 How Data Flows Through Our App

This is the most important section. Understanding this flow means you understand how web apps work.

### Example: User Adds a New Expense

Let's trace what happens when a user fills a form and clicks "Add Transaction":

```
Step 1: USER fills the form
┌──────────────────────────────────┐
│  Description: [Lunch at KFC    ] │
│  Amount:      [850             ] │
│  Type:        [Expense  ▼      ] │
│  Category:    [Food     ▼      ] │
│  Date:        [2026-02-15      ] │
│                                  │
│         [ Add Transaction ]      │
└──────────────────────────────────┘
        │
        │ User clicks the button
        ▼

Step 2: NEXT.JS SERVER ACTION runs
┌──────────────────────────────────┐
│  The form data is packaged into  │
│  a structured format:            │
│                                  │
│  {                               │
│    description: "Lunch at KFC",  │
│    amount: 850,                  │
│    type: "expense",              │
│    category: "food",             │
│    date: "2026-02-15"            │
│  }                               │
│                                  │
│  This is sent to FastAPI ───────►│
└──────────────────────────────────┘
        │
        ▼

Step 3: FASTAPI receives the data
┌──────────────────────────────────┐
│  FastAPI checks:                 │
│  ✓ Is amount a valid number?     │
│  ✓ Is type "income" or "expense"?│
│  ✓ Is the date valid?            │
│                                  │
│  If everything is OK, save to DB │
└──────────────────────────────────┘
        │
        ▼

Step 4: DATABASE saves the data
┌──────────────────────────────────┐
│  INSERT INTO transactions        │
│  (description, amount, type,     │
│   category, date)                │
│  VALUES ('Lunch at KFC', 850,    │
│   'expense', 'food',             │
│   '2026-02-15');                  │
│                                  │
│  ✓ Saved! Returns new ID: 5     │
└──────────────────────────────────┘
        │
        ▼

Step 5: RESPONSE travels back
┌──────────────────────────────────┐
│  Database → FastAPI → Server     │
│  Action → Browser                │
│                                  │
│  "Transaction added!"            │
│                                  │
│  The page refreshes and shows    │
│  the new transaction in the list │
└──────────────────────────────────┘
```

**This same flow happens for every action** — viewing transactions, editing them, or deleting them. The only things that change are:

- The HTTP method (GET, POST, PUT, DELETE)
- The data being sent
- What the database does with it

---

## 📁 Project Structure

Your project will have **two separate folders** — one for the frontend, one for the backend. They are separate applications that talk to each other.

```
personal-finance-tracker/
│
├── backend/                      ← FastAPI (Python, managed by uv)
│   ├── main.py                   ← The main file where your API lives
│   ├── database.py               ← Code to connect to Neon PostgreSQL
│   ├── models.py                 ← Defines what a "transaction" looks like
│   ├── schemas.py                ← Defines what data the API accepts/returns
│   ├── tests/                    ← Test files for your backend
│   │   ├── __init__.py
│   │   ├── test_transactions.py  ← Tests for transaction endpoints
│   │   └── test_summary.py       ← Tests for summary endpoint
│   ├── pyproject.toml            ← Project config & dependencies (managed by uv)
│   ├── uv.lock                   ← Locked dependency versions (auto-generated)
│   └── .env                      ← Secret stuff (database password) — NEVER share this
│
├── frontend/                     ← Next.js 16 (React)
│   ├── app/
│   │   ├── layout.tsx            ← The main layout (header, footer, etc.)
│   │   ├── page.tsx              ← The homepage / dashboard
│   │   ├── transactions/
│   │   │   └── page.tsx          ← The transactions list page
│   │   └── actions/
│   │       └── transactions.ts   ← Server Actions (functions that call FastAPI)
│   ├── components/
│   │   ├── TransactionForm.tsx   ← The form to add/edit a transaction
│   │   ├── TransactionList.tsx   ← The list of all transactions
│   │   ├── Dashboard.tsx         ← The summary cards & charts
│   │   └── PieChart.tsx          ← The spending breakdown chart
│   ├── __tests__/                ← Test files for your frontend
│   │   └── transactions.test.tsx ← Tests for transaction components
│   ├── package.json              ← List of JavaScript packages needed
│   └── .env.local                ← Secret stuff for frontend — NEVER share this
│
└── README.md                     ← Project description & how to run it
```

**Why two separate folders?** Because the frontend and backend are two different applications:

- The **backend** runs on Python (FastAPI + Uvicorn), managed by **uv**, usually on `http://localhost:8000`
- The **frontend** runs on JavaScript/TypeScript (Next.js 16 with Turbopack), usually on `http://localhost:3000`
- They communicate over HTTP (like two people talking over the phone)

---

## 🚀 Step-by-Step Guide: Building the App

### Phase 0: Setting Up the Foundation

Before writing any code, we need to set up our tools and services.

#### Step 0.1: Set Up Neon PostgreSQL Database

Neon gives you a free cloud database. Here's what to do:

1. Go to [https://neon.tech](https://neon.tech)
2. Sign up for a free account (you can use GitHub login)
3. Click **"Create a Project"**
4. Give your project a name: `finance-tracker`
5. Select the closest region to you
6. Click **Create Project**
7. Neon will show you a **connection string** — it looks something like this:
   ```
   postgresql://username:password@ep-something.region.aws.neon.tech/dbname?sslmode=require
   ```
8. **Copy this connection string** and save it somewhere safe — you'll need it for your backend

**What is a connection string?** It's like an address + key to your database. It contains:

- `username:password` → Who you are (authentication)
- `@ep-something.region.aws.neon.tech` → Where the database lives (address)
- `/dbname` → Which specific database to use
- `?sslmode=require` → Use a secure connection

#### Step 0.2: Set Up Your Project Folders (Run These Commands Yourself!)

Instead of asking Claude Code to create the folders, you'll run the setup commands yourself. This way you'll understand exactly what each tool creates and why.

**Open your terminal and follow these steps:**

##### Step 0.2a: Create the main project folder

```bash
mkdir personal-finance-tracker
cd personal-finance-tracker
```

This creates your root project folder and enters it. **This is where you will open VS Code and Claude Code.**

##### Step 0.2b: Set up the Backend with uv

```bash
# Initialize a new Python project with uv
uv init backend
```

**What just happened?** `uv init backend` created:

- `pyproject.toml` → The project configuration file (like a recipe card listing all ingredients your project needs)
- `.python-version` → Tells uv which Python version to use
- `hello.py` → A sample file (you can delete this later)

Now let's add the packages (dependencies) we need:

```bash
cd backend

# Add FastAPI with all its standard extras (includes uvicorn)
uv add "fastapi[standard]"

# Add SQLAlchemy (to talk to the database using Python)
uv add sqlalchemy

# Add psycopg2-binary (PostgreSQL driver - lets Python connect to PostgreSQL)
uv add psycopg2-binary

# Add python-dotenv (to read .env files)
uv add python-dotenv

# Add alembic (for database migrations - optional but recommended)
uv add alembic
```

**What just happened?** Each `uv add` command:

1. Downloaded and installed the package (super fast!)
2. Added it to `pyproject.toml` under `[project] dependencies`
3. Updated `uv.lock` (a lockfile that records exact versions)
4. Created a `.venv` folder (virtual environment) automatically

Now look at your folder:

```
backend/
├── .venv/              ← Virtual environment (auto-created by uv)
├── .python-version     ← Python version
├── pyproject.toml      ← Your project config with all dependencies
├── uv.lock             ← Exact locked versions of all packages
└── hello.py            ← Sample file (delete this)
```

Go back to the root folder:

```bash
cd ..
```

##### Step 0.2c: Set up the Frontend with Next.js 16

```bash
# Create a new Next.js 16 project in a folder called "frontend"
npx create-next-app@latest frontend
```

**You'll be asked some questions. Choose these options:**

```
Would you like to use TypeScript?                    → Yes
Would you like to use ESLint?                        → Yes
Would you like to use Tailwind CSS?                  → Yes
Would you like your code inside a `src/` directory?  → No
Would you like to use App Router? (recommended)      → Yes
Would you like to use Turbopack for next dev?        → Yes
Would you like to customize the import alias?        → No
```

**What just happened?** `create-next-app` created a complete Next.js 16 project with:

- `app/` folder → Where your pages live (App Router)
- `public/` folder → For static files (images, etc.)
- `package.json` → Lists all JavaScript dependencies (like `pyproject.toml` for Python)
- `next.config.ts` → Next.js configuration
- `tailwind.config.ts` → Tailwind CSS configuration
- `tsconfig.json` → TypeScript configuration
- `node_modules/` → All installed JavaScript packages (like `.venv` for Python)

Now look at your complete project structure:

```
personal-finance-tracker/
├── backend/          ← Created with uv init
│   ├── .venv/
│   ├── pyproject.toml
│   ├── uv.lock
│   └── ...
└── frontend/         ← Created with create-next-app
    ├── app/
    ├── node_modules/
    ├── package.json
    └── ...
```

**🎉 Both projects are now set up!** You can verify by running:

```bash
# Test backend (from the backend/ folder)
cd backend
uv run python -c "import fastapi; print(f'FastAPI {fastapi.__version__} installed!')"
cd ..

# Test frontend (from the frontend/ folder)
cd frontend
npm run dev
# You should see: ▲ Next.js 16 — then press Ctrl+C to stop
cd ..
```

#### Step 0.3: Understanding the `.env` File

Your database connection string is **secret** — it contains your password. We NEVER put passwords directly in our code. Instead, we put them in a `.env` file:

```
# backend/.env
DATABASE_URL=postgresql://username:password@ep-something.region.aws.neon.tech/dbname?sslmode=require
```

And in the code, we read from this file instead of hardcoding the password. This way:

- If you share your code on GitHub, the password isn't exposed
- You can change the password without changing your code

**⚠️ CRITICAL:** Add `.env` to your `.gitignore` file so it's never uploaded to GitHub.

---

### Phase 1: Build the Backend (FastAPI)

We start with the backend because it's the foundation. The frontend needs the backend to work, but the backend can work on its own.

#### Step 1.1: Set Up the Database Connection

**What you're doing:** Writing code that connects your Python application to your Neon PostgreSQL database.

**Why:** Your app needs to talk to the database to save and retrieve transactions. This file creates that connection.

**What to tell Claude Code:**

> "Set up a database connection in `backend/database.py` using SQLAlchemy to connect to my Neon PostgreSQL database. The connection string should come from a `.env` file using `DATABASE_URL` environment variable. I'm using uv for package management — all dependencies are already in pyproject.toml."

**What is SQLAlchemy?** It's a Python library that lets you interact with databases using Python code instead of raw SQL. Think of it as a translator between Python and the database. (You already installed it with `uv add sqlalchemy` in Step 0.2b!)

#### Step 1.2: Define the Data Model

**What you're doing:** Telling the database what a "transaction" looks like — what columns it has and what type of data each column holds.

**Why:** Just like you need to design the columns of an Excel sheet before entering data, you need to tell the database the structure of your data.

**Each transaction should have:**

| Field       | Type     | Description                        | Example        |
| ----------- | -------- | ---------------------------------- | -------------- |
| id          | Integer  | Unique identifier (auto-generated) | 1, 2, 3...     |
| description | String   | What the transaction was for       | "Lunch at KFC" |
| amount      | Float    | How much money                     | 850.00         |
| type        | String   | "income" or "expense"              | "expense"      |
| category    | String   | Category of spending               | "food"         |
| date        | Date     | When the transaction happened      | "2026-02-15"   |
| created_at  | DateTime | When the record was created        | Auto-generated |

**What to tell Claude Code:**

> "Create a SQLAlchemy model in `backend/models.py` for a Transaction with these fields: id (primary key, auto-increment), description (string, required), amount (float, required), type (string, either 'income' or 'expense'), category (string, required), date (date), created_at (datetime, auto-set to now)."

#### Step 1.3: Create Pydantic Schemas

**What you're doing:** Defining the "rules" for what data your API accepts and returns.

**Why:** When someone sends data to your API, you want to make sure it's valid. What if someone sends a negative amount? Or forgets the description? Schemas catch these problems automatically.

**What is Pydantic?** A Python library that validates data. You define what the data should look like, and Pydantic checks incoming data against those rules.

```
Without Pydantic:
User sends: { amount: "not a number" }  →  Your app crashes  💥

With Pydantic:
User sends: { amount: "not a number" }  →  Pydantic: "Error: amount must be a number"  ✓
```

**What to tell Claude Code:**

> "Create Pydantic schemas in `backend/schemas.py` for:
>
> - `TransactionCreate` — the data needed to create a transaction (description, amount, type, category, date)
> - `TransactionUpdate` — for updating (all fields optional)
> - `TransactionResponse` — what the API returns (includes id and created_at)"

#### Step 1.4: Build the API Endpoints

**What you're doing:** Creating the actual URLs that the frontend will call to create, read, update, and delete transactions.

**Why:** This is the core of your backend — the "waiter" that takes orders from the frontend and brings back results.

**The endpoints you need:**

```
┌─────────────────────────────────────────────────────────────────────┐
│                        API ENDPOINTS                                │
├──────────┬──────────────────────┬────────────────────────────────────┤
│  Method  │  URL                 │  What It Does                     │
├──────────┼──────────────────────┼────────────────────────────────────┤
│  GET     │  /transactions       │  Get all transactions             │
│          │                      │  (with optional filters)          │
├──────────┼──────────────────────┼────────────────────────────────────┤
│  GET     │  /transactions/{id}  │  Get one specific transaction     │
├──────────┼──────────────────────┼────────────────────────────────────┤
│  POST    │  /transactions       │  Create a new transaction         │
├──────────┼──────────────────────┼────────────────────────────────────┤
│  PUT     │  /transactions/{id}  │  Update a transaction             │
├──────────┼──────────────────────┼────────────────────────────────────┤
│  DELETE  │  /transactions/{id}  │  Delete a transaction             │
├──────────┼──────────────────────┼────────────────────────────────────┤
│  GET     │  /summary            │  Get totals (income, expense,     │
│          │                      │  balance, category breakdown)     │
└──────────┴──────────────────────┴────────────────────────────────────┘
```

**What to tell Claude Code:**

> "Create FastAPI endpoints in `backend/main.py`:
>
> 1. GET `/transactions` — returns all transactions, with optional query parameters to filter by type, category, and date range
> 2. GET `/transactions/{id}` — returns a single transaction
> 3. POST `/transactions` — creates a new transaction
> 4. PUT `/transactions/{id}` — updates an existing transaction
> 5. DELETE `/transactions/{id}` — deletes a transaction
> 6. GET `/summary` — returns total income, total expense, balance, and category-wise breakdown
>
> Use SQLAlchemy for database operations. Include CORS middleware so the frontend can connect."

**What is CORS?** When your frontend (localhost:3000) tries to talk to your backend (localhost:8000), the browser blocks it by default for security. CORS (Cross-Origin Resource Sharing) is a setting that tells the browser "it's OK, this frontend is allowed to talk to me."

#### Step 1.5: Test Your Backend

**What you're doing:** Making sure your API works before building the frontend.

**Why:** It's much easier to find and fix problems when you test one part at a time.

**How to run the backend:**

1. Navigate to the backend folder: `cd backend`
2. Sync all dependencies: `uv sync` (this installs everything from `pyproject.toml` — like `pip install -r requirements.txt` but faster and smarter)
3. Run the server: `uv run uvicorn main:app --reload`
4. Open your browser and go to: `http://localhost:8000/docs`

**Why `uv run`?** It runs the command inside the project's virtual environment automatically. Without `uv run`, you'd have to manually activate the `.venv` first. `uv run` handles that for you.

**What you'll see at `/docs`:** FastAPI automatically generates a beautiful, interactive documentation page. You can:

- See all your endpoints listed
- Click on any endpoint to expand it
- Click "Try it out" to test it directly
- See what data it expects and returns

```
┌─────────────────────────────────────────────────────────────┐
│  FastAPI - Swagger UI                    http://localhost:8000/docs  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  GET    /transactions        List all transactions    ▼     │
│  POST   /transactions        Create a transaction     ▼     │
│  GET    /transactions/{id}   Get one transaction      ▼     │
│  PUT    /transactions/{id}   Update a transaction     ▼     │
│  DELETE /transactions/{id}   Delete a transaction     ▼     │
│  GET    /summary             Get financial summary    ▼     │
│                                                             │
│  Click any endpoint → "Try it out" → Test it!               │
└─────────────────────────────────────────────────────────────┘
```

**🎉 Checkpoint:** At this point, your backend is done! You have a working API. Even without a frontend, you can create, read, update, and delete transactions through the `/docs` page.

---

### Phase 2: Build the Frontend (Next.js)

Now that the backend is working, we build what the user actually sees.

#### Step 2.1: Explore Your Next.js 16 Project

**What you're doing:** You already created the Next.js 16 project in Step 0.2c. Now let's understand what was created and make sure Claude Code knows about it.

**Open the `frontend` folder and look at the key files:**

- `app/page.tsx` → This is your homepage. Everything you see when you open `http://localhost:3000`
- `app/layout.tsx` → The "wrapper" around every page (where you put the navigation bar)
- `next.config.ts` → Settings for Next.js (TypeScript config file — new in v16!)
- `package.json` → Lists all JavaScript packages (like `pyproject.toml` for Python)
- `tailwind.config.ts` → Settings for Tailwind CSS

**What is TypeScript?** It's JavaScript with type checking. Don't worry — Claude Code handles this for you. Just know that `.tsx` files are like `.jsx` files but with type safety.

**What is Tailwind CSS?** Instead of writing CSS in separate files, you add styling directly to your HTML elements using class names like `bg-blue-500`, `text-white`, `p-4`. It's like using shorthand instead of writing full sentences.

**What is Turbopack?** In Next.js 16, Turbopack is the default bundler. A bundler takes all your code files and combines them into optimized files the browser can use. Turbopack is **5-10x faster** than the old bundler (Webpack). You don't need to configure anything — it just works when you run `npm run dev`.

#### Step 2.2: Create Server Actions

**What you're doing:** Writing functions that run on the Next.js server and call your FastAPI backend.

**Why:** These functions are the bridge between what the user does (clicks a button) and what your backend does (saves to database).

```
HOW SERVER ACTIONS WORK:

User clicks "Add" → Server Action runs → Calls FastAPI → Gets response → Updates UI

The Server Action is just a function, for example:

  async function addTransaction(data) {
      // This runs on the SERVER, not in the browser
      const response = await fetch('http://localhost:8000/transactions', {
          method: 'POST',
          body: JSON.stringify(data)
      });
      return response;
  }
```

**What to tell Claude Code:**

> "Create Server Actions in `frontend/app/actions/transactions.ts` with these functions:
>
> - `getTransactions()` — fetches all transactions from FastAPI
> - `getTransactionById(id)` — fetches one transaction
> - `addTransaction(data)` — sends new transaction to FastAPI
> - `updateTransaction(id, data)` — updates a transaction
> - `deleteTransaction(id)` — deletes a transaction
> - `getSummary()` — fetches the financial summary
>
> Each function should call the corresponding FastAPI endpoint at `http://localhost:8000`."

#### Step 2.3: Build the Dashboard Page

**What you're doing:** Creating the main page users see when they open the app.

**The dashboard should show:**

```
┌─────────────────────────────────────────────────────────────────┐
│  💰 Personal Finance Tracker                                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   INCOME    │  │  EXPENSES   │  │   BALANCE   │             │
│  │             │  │             │  │             │             │
│  │  Rs 65,000  │  │  Rs 23,500  │  │  Rs 41,500  │             │
│  │  ↑ Green    │  │  ↓ Red      │  │  Blue       │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              SPENDING BY CATEGORY                        │   │
│  │                                                          │   │
│  │         ████  Food: 35%                                  │   │
│  │      ████████  Transport: 25%                            │   │
│  │    ██████████  Shopping: 20%                              │   │
│  │      ████████  Bills: 15%                                │   │
│  │         ████  Other: 5%                                  │   │
│  │                                                          │   │
│  │              (Pie/Donut Chart)                            │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  RECENT TRANSACTIONS                [ + Add New ]        │   │
│  ├──────────────────────────────────────────────────────────┤   │
│  │  🔴 Lunch at KFC       -Rs 850    Food    Feb 15, 2026   │   │
│  │  🔴 Uber ride          -Rs 350    Transport Feb 14       │   │
│  │  🟢 Salary            +Rs 50,000  Salary   Feb 01       │   │
│  │  🔴 Groceries         -Rs 2,500   Food     Feb 02       │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**What to tell Claude Code:**

> "Build the dashboard page at `frontend/app/page.tsx` that:
>
> 1. Shows three summary cards at the top: Total Income (green), Total Expenses (red), Balance (blue)
> 2. Shows a pie/donut chart for category-wise spending breakdown
> 3. Shows a list of recent transactions with color-coding (green for income, red for expense)
> 4. Has a button to add a new transaction
> 5. Uses the Server Actions to fetch data from the backend
> 6. Use Tailwind CSS for styling and any chart library like recharts for the pie chart"

#### Step 2.4: Build the Transaction Form

**What you're doing:** Creating a form that lets users add or edit transactions.

**What to tell Claude Code:**

> "Create a transaction form component at `frontend/components/TransactionForm.tsx` that:
>
> 1. Has fields for: description, amount, type (income/expense dropdown), category (dropdown with options: food, transport, shopping, bills, salary, freelance, entertainment, health, education, other), and date
> 2. When submitted, calls the `addTransaction` Server Action
> 3. If editing, pre-fills the form and calls `updateTransaction`
> 4. Shows validation errors (amount must be positive, description required)
> 5. After successful submission, refreshes the transaction list"

#### Step 2.5: Build the Transaction List with Filters

**What you're doing:** Showing all transactions with the ability to filter and search.

**What to tell Claude Code:**

> "Create a transaction list page at `frontend/app/transactions/page.tsx` that:
>
> 1. Shows all transactions in a clean table or card layout
> 2. Has filter options: by type (all/income/expense), by category, by date range
> 3. Each transaction has edit and delete buttons
> 4. Delete shows a confirmation dialog before deleting
> 5. Edit opens the transaction form pre-filled with data
> 6. Uses Server Actions for all data operations"

#### Step 2.6: Add Navigation

**What you're doing:** Adding a navigation bar so users can move between pages (Dashboard and Transactions).

**What to tell Claude Code:**

> "Add a navigation bar in `frontend/app/layout.tsx` with links to:
>
> 1. Dashboard (home page `/`)
> 2. All Transactions (`/transactions`)
> 3. Add Transaction (`/transactions/new`)
>    The nav should highlight the current active page."

---

### Phase 3: Connect Everything & Test

#### Step 3.1: Run Both Servers Together

You need **two terminal windows** running at the same time:

```
Terminal 1 (Backend):
┌──────────────────────────────────────┐
│  cd backend                          │
│  uv run uvicorn main:app --reload    │
│                                      │
│  INFO: Uvicorn running on            │
│  http://127.0.0.1:8000               │
│  ✓ Backend is running!               │
└──────────────────────────────────────┘

Terminal 2 (Frontend):
┌──────────────────────────────────────┐
│  cd frontend                         │
│  npm run dev                         │
│                                      │
│  ▲ Next.js 16 (Turbopack)            │
│  - Local: http://localhost:3000      │
│  ✓ Frontend is running!              │
└──────────────────────────────────────┘
```

Now open `http://localhost:3000` in your browser — you should see your app!

#### Step 3.2: Test the Full Flow

Test each feature in this order:

1. **Add a transaction** → Does it appear in the list?
2. **Add multiple transactions** (both income and expense) → Does the dashboard update?
3. **Check the pie chart** → Does it show category breakdown?
4. **Edit a transaction** → Does it update correctly?
5. **Delete a transaction** → Does it disappear?
6. **Filter transactions** → Do filters work correctly?
7. **Check summary cards** → Are totals calculated correctly?

---

### Phase 4: Writing Tests — Making Sure Nothing Breaks

#### What Are Tests?

Imagine you build a calculator app. You click "2 + 2" and it shows 5. That's a bug. You fix it. But tomorrow you add a new feature, and now "2 + 2" shows 5 again. **How do you catch this?**

Tests are small programs that **automatically check if your code works correctly.** Instead of you manually clicking every button after every change, tests do it for you in seconds.

```
WITHOUT Tests:
┌──────────────────────────────────────────────────────┐
│  You change some code                                │
│      │                                               │
│      ▼                                               │
│  You manually test the whole app                     │
│  (open browser, click every button, fill forms...)   │
│      │                                               │
│      ▼                                               │
│  Takes 20 minutes. You miss a bug anyway.  😩        │
└──────────────────────────────────────────────────────┘

WITH Tests:
┌──────────────────────────────────────────────────────┐
│  You change some code                                │
│      │                                               │
│      ▼                                               │
│  Run: uv run pytest (backend)                        │
│  Run: npm test (frontend)                            │
│      │                                               │
│      ▼                                               │
│  ✓ 15 tests passed in 3 seconds!                     │
│  ✗ 1 test failed: "Delete endpoint returns 500"      │
│                                                      │
│  You know EXACTLY what broke.  🎯                    │
└──────────────────────────────────────────────────────┘
```

**Think of tests like a checklist that runs automatically.** Every time you make a change, you run the checklist, and it tells you if anything broke.

#### Why Do We Write Tests?

1. **Confidence:** You can change code without fear of breaking things
2. **Documentation:** Tests show how your code is supposed to work
3. **Catching bugs early:** Find problems in 3 seconds instead of after users complain
4. **Professional standard:** Every real company requires tests. Having tested code on your portfolio shows maturity.

#### Types of Tests We'll Write

| Type                           | What It Tests                          | Example                                                      |
| ------------------------------ | -------------------------------------- | ------------------------------------------------------------ |
| **Unit Test**            | One small piece of code in isolation   | "Does the summary calculation add numbers correctly?"        |
| **API/Integration Test** | That your API endpoints work correctly | "Does POST `/transactions` actually create a transaction?" |
| **Component Test**       | That a UI component renders correctly  | "Does the Dashboard show three summary cards?"               |

#### Step 4.1: Write Backend Tests (pytest)

**What is pytest?** A Python testing framework. It's the most popular way to test Python code. You write functions that start with `test_`, and pytest finds and runs them all.

First, add pytest to your backend project:

```bash
cd backend
uv add --dev pytest httpx
```

**Why `--dev`?** The `--dev` flag tells uv that pytest is a **development dependency** — you need it while building the app, but users of your app don't need it. It keeps your production dependencies clean.

**What is httpx?** It's like a fake browser that can send requests to your FastAPI app during tests, without actually starting the server.

Now tell Claude Code to write the tests:

> **Prompt to Claude Code:**
> "Write pytest tests for my FastAPI backend in `backend/tests/test_transactions.py`. The tests should cover:
>
> 1. Creating a transaction via POST `/transactions` and checking it returns 201
> 2. Getting all transactions via GET `/transactions`
> 3. Getting a single transaction by ID
> 4. Updating a transaction via PUT `/transactions/{id}`
> 5. Deleting a transaction via DELETE `/transactions/{id}`
> 6. Trying to create a transaction with invalid data (negative amount) and checking it returns 422
> 7. Testing the GET `/summary` endpoint returns correct totals
>
> Use httpx.AsyncClient with FastAPI's TestClient. Use a test database (SQLite in-memory) so tests don't affect real data."

**How a test looks (conceptually):**

```
Test: "Creating a transaction should work"
  1. Send a POST request with: description="Lunch", amount=500, type="expense"
  2. Check: Did the response status code = 201 (created)?
  3. Check: Does the response contain an "id"?
  4. Check: Is the description "Lunch"?
  → If all checks pass: ✅ TEST PASSED
  → If any check fails: ❌ TEST FAILED (tells you exactly which check failed)
```

**Run the tests:**

```bash
cd backend
uv run pytest -v
```

The `-v` flag means "verbose" — it shows you each test name and whether it passed or failed:

```
$ uv run pytest -v
========================= test session starts ==========================
test_transactions.py::test_create_transaction         PASSED  ✅
test_transactions.py::test_get_all_transactions       PASSED  ✅
test_transactions.py::test_get_transaction_by_id      PASSED  ✅
test_transactions.py::test_update_transaction          PASSED  ✅
test_transactions.py::test_delete_transaction          PASSED  ✅
test_transactions.py::test_invalid_amount              PASSED  ✅
test_summary.py::test_summary_calculations             PASSED  ✅
========================== 7 passed in 1.2s ============================
```

**🎉 All green!** If any test shows FAILED, read the error message — it tells you exactly what went wrong.

#### Step 4.3: Make All Tests Pass

Here's the important part: **don't just write tests — make sure they pass!**

If tests fail, work with Claude Code to fix the issues:

> **Prompt to Claude Code:**
> "My test `test_create_transaction` is failing with this error: [paste error]. Can you help me fix it?"

The goal is:

- **Backend:** All pytest tests pass (`uv run pytest -v` shows all green)
- THE TESTING WORKFLOW:

---

## 📂 Categories to Include

Your app should support these transaction categories:

| Category      | Icon Suggestion | Used For                     |
| ------------- | --------------- | ---------------------------- |
| Food          | 🍕              | Groceries, restaurants       |
| Transport     | 🚗              | Uber, fuel, bus fare         |
| Shopping      | 🛍️            | Clothes, electronics         |
| Bills         | 📄              | Electricity, internet, phone |
| Entertainment | 🎬              | Movies, Netflix, games       |
| Health        | 🏥              | Medicine, doctor visits      |
| Education     | 📚              | Books, courses               |
| Salary        | 💼              | Monthly salary               |
| Freelance     | 💻              | Side income                  |
| Other         | 📦              | Anything else                |

---

## ✅ Functional Requirements Checklist

### Must-Have (Core Features):

- [ ] **Add Transaction:** User can add a transaction with description, amount, type (income/expense), category, and date
- [ ] **View All Transactions:** User can see a list of all transactions
- [ ] **Edit Transaction:** User can modify any existing transaction
- [ ] **Delete Transaction:** User can delete a transaction (with confirmation)
- [ ] **Dashboard Summary Cards:** Show total income, total expenses, and balance
- [ ] **Category Breakdown Chart:** Pie or donut chart showing spending by category
- [ ] **Filter Transactions:** Filter by type (income/expense) and by category
- [ ] **Backend API Docs:** FastAPI `/docs` page is accessible and shows all endpoints
- [ ] **Data Persistence:** All data is stored in Neon PostgreSQL (survives server restart)
- [ ] **Backend Tests:** At least 5 pytest tests covering CRUD operations, all passing
- [ ] **Frontend Tests:** At least 3 Vitest tests covering component rendering, all passing

### Nice-to-Have (Bonus Features):

- [ ] **Date Range Filter:** Filter transactions by date range (e.g., "this month")
- [ ] **Monthly Bar Chart:** Show income vs expenses for each month
- [ ] **Search:** Search transactions by description
- [ ] **Budget Limits:** Set a budget per category and show warning when exceeded
- [ ] **Export to CSV:** Download all transactions as a CSV file
- [ ] **Responsive Design:** App looks good on mobile screens too
- [ ] **Dark Mode:** Toggle between light and dark themes

---

## 📊 Evaluation Criteria

| Criteria                                              | Points        |
| ----------------------------------------------------- | ------------- |
| **Backend API (FastAPI + uv)**                  |               |
| All CRUD endpoints work correctly                     | 15            |
| Summary endpoint with correct calculations            | 10            |
| Pydantic validation catches invalid data              | 5             |
| Database connection and models set up correctly       | 10            |
| Project uses uv (`pyproject.toml` + `uv.lock`)    | 5             |
| **Frontend (Next.js 16)**                       |               |
| Dashboard with summary cards displays correctly       | 10            |
| Pie/donut chart shows category breakdown              | 10            |
| Transaction form works (add & edit)                   | 10            |
| Transaction list with delete functionality            | 5             |
| Filters work correctly                                | 5             |
| **Integration & Server Actions**                |               |
| Server Actions correctly call FastAPI                 | 10            |
| Data flows correctly between frontend and backend     | 5             |
| **Testing**                                     |               |
| Backend: pytest tests exist and pass                  | 10            |
|                                                       |               |
| **Code Quality & Understanding**                |               |
| Clean project structure                               | 10            |
| README with project description & setup steps         | 5             |
| Student can explain how data flows in their app       | 5             |
| **Subtotal (Base Assignment)**                  | **130** |
| **Bonus Features (any from Nice-to-Have list)** | **+20** |
| **TOTAL POSSIBLE**                              | **150** |

---

## 📝 Submission Requirements

Submit a **GitHub repository** containing:

1. ✅ **Complete source code** (both `backend/` and `frontend/` folders)
2. ✅ **README.md** with:
   - Project name and description
   - Screenshots of your app (dashboard, form, transaction list)
   - How to set up and run the project (step-by-step, using `uv` and `npm`)
   - How to run the tests (`uv run pytest -v` and `npx vitest run`)
   - What technologies you used and why
   - How you used Claude Code (be specific about what prompts you gave)
3. ✅ **`.env.example`** file showing what environment variables are needed (without actual passwords)
4. ✅ **Working app** — The instructors will clone your repo and try to run it
5. ✅ **Passing tests** — All backend tests must pass
6. ⚠️ **`.gitignore`** must exclude `.env` files, `node_modules`, and `.venv`

**The instructor should be able to run your project with just these commands:**

```bash
# Backend
cd backend
uv sync
uv run uvicorn main:app --reload

# Frontend (in another terminal)
cd frontend
npm install
npm run dev

# Tests
cd backend && uv run pytest -v
```

---

## 💡 Tips for Working with Claude Code

1. **Build incrementally.** Don't try to prompt Claude Code for the entire app at once. Follow the phases — backend first, then frontend, then tests.
2. **Test after each step.** After each phase, make sure it works before moving on. It's much harder to debug everything at the end.
3. **Be specific in your prompts.** Instead of "build me a finance app," tell Claude Code exactly what you want:

   > "Create a FastAPI endpoint that accepts a POST request at `/transactions` with fields: description (string), amount (float), type (string), category (string), date (date). Validate that amount is positive and type is either 'income' or 'expense'. Save it to the PostgreSQL database using SQLAlchemy. I'm using uv for package management."
   >
4. **Understand what Claude Code generates.** After Claude generates code, read through it and make sure you can explain what each part does. If you don't understand something, ask Claude Code to explain it.
5. **Use `/docs` to test your backend.** Before building any frontend, make sure your API works by testing it through FastAPI's built-in documentation page.
6. **Keep your terminal open.** Watch for errors in both the backend and frontend terminals. Error messages tell you exactly what's wrong.
7. **Write tests as you go.** After building each feature, ask Claude Code to write tests for it. Don't leave all testing for the end.
8. **Use `uv run` for everything Python.** Never run `python` or `pip` directly. Always use `uv run python ...`, `uv run pytest`, etc. This ensures you're always using the correct virtual environment.

---

## ⚠️ Common Mistakes to Avoid

| Mistake                                         | Why It Happens                               | How to Avoid                                               |
| ----------------------------------------------- | -------------------------------------------- | ---------------------------------------------------------- |
| Forgetting CORS                                 | Browser blocks frontend from calling backend | Ask Claude Code to add CORS middleware in FastAPI          |
| Exposing `.env` on GitHub                     | Not adding to `.gitignore`                 | Create `.gitignore` FIRST before your first commit       |
| Backend not running when testing frontend       | Only started one server                      | Always run both terminals                                  |
| Database connection errors                      | Wrong connection string                      | Double-check the Neon connection string in `.env`        |
| Port conflicts                                  | Another app using port 8000 or 3000          | Kill the other process or use a different port             |
| Using `pip install` instead of `uv add`     | Old habit                                    | Always use `uv add <package>` to install Python packages |
| Running `python` instead of `uv run python` | Forgetting uv manages the environment        | Always prefix Python commands with `uv run`              |
| Tests pass locally but code is broken           | Tests don't cover enough cases               | Write tests for both happy paths AND error cases           |
| Forgetting `.venv` in `.gitignore`          | Not knowing uv creates it                    | Add `.venv/` to `.gitignore` along with `.env`       |

---

## 🔗 Useful Resources

**Backend:**

- **FastAPI Documentation:** [https://fastapi.tiangolo.com](https://fastapi.tiangolo.com)
- **uv Documentation:** [https://docs.astral.sh/uv](https://docs.astral.sh/uv)
- **uv + FastAPI Guide:** [https://docs.astral.sh/uv/guides/integration/fastapi](https://docs.astral.sh/uv/guides/integration/fastapi)
- **SQLAlchemy Tutorial:** [https://docs.sqlalchemy.org/en/20/tutorial](https://docs.sqlalchemy.org/en/20/tutorial)
- **pytest Documentation:** [https://docs.pytest.org](https://docs.pytest.org)
- **Neon PostgreSQL:** [https://neon.tech/docs](https://neon.tech/docs)

**Frontend:**

- **Next.js 16 Blog Post:** [https://nextjs.org/blog/next-16](https://nextjs.org/blog/next-16)
- **Next.js Documentation:** [https://nextjs.org/docs](https://nextjs.org/docs)
- **Next.js Server Actions:** [https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)
- **Vitest Documentation:** [https://vitest.dev](https://vitest.dev)
- **React Testing Library:** [https://testing-library.com/docs/react-testing-library/intro](https://testing-library.com/docs/react-testing-library/intro)
- **Recharts (for charts):** [https://recharts.org](https://recharts.org)
- **Tailwind CSS:** [https://tailwindcss.com/docs](https://tailwindcss.com/docs)

---

## 🏁 Summary: What You'll Learn

By completing this assignment, you will understand:

| Concept                                                 | How You'll Learn It                           |
| ------------------------------------------------------- | --------------------------------------------- |
| How frontends and backends communicate                  | By connecting Next.js 16 to FastAPI           |
| What APIs are and how they work                         | By building and testing FastAPI endpoints     |
| How databases store data                                | By using Neon PostgreSQL                      |
| How data validation works                               | By using Pydantic schemas                     |
| How Server Actions simplify frontend-backend connection | By writing server actions in Next.js 16       |
| How modern Python projects are managed                  | By using uv for dependencies and environments |
| How to write and run automated tests                    | By writing pytest and Vitest tests            |
| Why testing matters in real projects                    | By catching bugs before they reach users      |
| How to structure a real project                         | By organizing code into frontend & backend    |
| How to read error messages and debug                    | By testing at each step                       |
| How to use AI coding tools effectively                  | By prompting Claude Code strategically        |

You're not just building a finance tracker — you're learning how **every web application in the world** is built. The same patterns (frontend → API → database) are used by Instagram, Amazon, Uber, and every other app you use daily.

**Good luck! 🚀**
