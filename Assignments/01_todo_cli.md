# Assignment 1: Python CLI Todo App

*with Claude Code Assistance*

## 1. Overview

This assignment is a beginner-friendly version of a more advanced full-stack Todo project. In this track, you will:

- Build a Todo List application in the command line (CLI) using Python
- Use Claude Code as your AI coding assistant
- Focus on core programming concepts
- Work entirely in your terminal using Python

This hackathon emphasizes learning by building, experimenting, and problem-solving with AI support.

## 2. Target Participants & Prerequisites

This hackathon track is designed for students who:

- Are beginners in programming
- Know basic Python syntax (variables, lists, functions, loops)
- Have access to Claude Code and know how to ask it for help

## 3. Problem Statement

Build a Python command-line Todo application that helps a user manage tasks from the terminal. The user should be able to:

1. Add new todo tasks
2. View/List all tasks
3. Mark tasks as complete or incomplete
4. Update/Edit an existing task (e.g., change its text)
5. Delete a task

The app should be simple to run:

```
python main.py
```

And show a menu like:

```
==== TODO APP ====
1. Add task
2. List tasks
3. Mark task complete
4. Update task
5. Delete task
0. Exit
Choose an option:
```

## 4. Functional Requirements

### 4.1 Add Task

- Ask the user to enter a task title (e.g., "Study Python for 1 hour")
- Optionally, also ask for:
  - Short description
  - Priority (Low / Medium / High) or due date

Store each task as a Python object, for example:

- A dictionary: `{"id": 1, "title": "Study Python", "done": False}`
- Or a class

### 4.2 List Tasks

- Show all tasks with:
  - ID (index or number)
  - Title
  - Status (Done / Not Done)

Example:

```
[1] Study Python for 1 hour    [Not Done]
[2] Complete math assignment  [Done]
```

### 4.3 Mark Task Complete / Incomplete

- Ask the user which task (by ID or index) to update
- Toggle the status:
  - Not Done → Done
  - Done → Not Done

### 4.4 Update Task

- Allow the user to change the title (and description if present)

Example flow:

```
Enter task ID to update: 2
Current title: "Go for a walk"
Enter new title (leave blank to keep same): Go for a long walk
```

### 4.5 Delete Task

- Ask for the task ID
- Remove it from the list
- Show a confirmation message

## 5. Data Storage Requirements

Participants may choose Level 1 or Level 2 based on comfort level.

### Level 1 – In-Memory Only (Minimum Requirement)

- Tasks stored in a Python list while the program runs
- Data is lost when the program exits
- This is perfectly acceptable for beginners

### Level 2 – File-Based Storage (Bonus)

- Save tasks to a file (e.g., tasks.json) whenever the user:
  - Adds / updates / deletes tasks
  - Exits the app
- Load tasks from the file on startup
- You may use:
  - Python's json module

## 6. Non-Functional Expectations

### 1. Clean Code

- Use functions instead of writing everything in main.py
- Example functions:
  - `add_task(tasks)`
  - `list_tasks(tasks)`
  - `update_task(tasks)`
  - `delete_task(tasks)`
  - `save_tasks(tasks)`
  - `load_tasks()`

### 2. Basic Error Handling

- Handle invalid menu options gracefully
- Handle invalid task IDs without crashing

### 3. User-Friendly Messages

- Clear menus and prompts
- Helpful feedback, such as:
  - "Task added successfully."
  - "No tasks found."
  - "Invalid option, please try again."

## 7. Use of Claude Code

You are allowed and encouraged to use Claude Code as your AI coding assistant during the hackathon. You can ask Claude Code to:

- Help design functions and program structure
- Generate a starter version of the app
- Debug errors and exceptions
- Suggest refactoring or improvements

However:

- You are responsible for understanding the code
- You should read, modify, and test what Claude generates
- Blind copy-paste without understanding may negatively affect evaluation

You must mention in your README:

- How you used Claude Code (e.g., "Claude Code helped generate the initial menu loop and task structure.")

## 8. Project Structure

A recommended simple structure:

```
todo-cli/
├── main.py
├── tasks.py        (optional: task logic)
├── storage.py      (optional: file handling)
└── README.md
```

## 9. Submission Requirements

Each participant (or team, if applicable) must submit:

1. Code (GitHub repository) containing:
   - All project files
   - README.md with:
     - Project name
     - Short description of the app
     - How to run the app:
       ```
       python main.py
       ```
     - Any bonus features implemented
     - A brief explanation of how Claude Code was used

## 10. Evaluation Criteria

Judging will be based on:

1. **Core Functionality (40%)**
   - Add / List / Update / Delete / Mark Complete works correctly
   - Menu loop functions without crashing

2. **Code Quality (25%)**
   - Clear functions and readable logic
   - Meaningful variable names
   - Basic comments where helpful
   - Reasonable file structure

3. **CLI User Experience (15%)**
   - Clean menu layout
   - Clear prompts and messages
   - Easy for non-technical users

4. **Use of Python Features (10%)**
   - Proper use of lists/dicts or classes
   - Optional: file handling, modules

5. **Bonus Features & Creativity (10%)**
   - File-based persistence
   - Task filtering or sorting
   - Priority levels
   - Clean formatting
   - Relatability to real-life use

## 11. Optional Bonus Ideas (For Fast Learners)

Not required, but encouraged:

- Search tasks by keyword
- Filters:
  - Show only completed tasks
  - Show only pending tasks
- Due dates or completion dates (simple strings)
- Colorful CLI output
- SQLite-based persistence
