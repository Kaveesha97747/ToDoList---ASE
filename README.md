# Todo List

A simple, responsive Todo List application built using **Vue 3**, **Tailwind CSS**, and **LocalStorage**.  
This project was completed as part of a frontend assessment task (ASE Task).

---

## Features

- **Add/Edit/Delete Todos**  
  Create new tasks, update existing ones, and remove completed or unnecessary tasks.

- **Complete/Uncomplete Todos**  
  Mark tasks as done or pending with a single click.

- **Filters**  
  Easily switch between `All`, `Completed`, and `Pending` tasks.

- **Due Dates & Description**  
  Optionally add descriptions and due dates for each todo.

- **LocalStorage Persistence**  
  Todos are saved in the browser and persist across page reloads.

- **Responsive Design**  
  Mobile-first layout, works seamlessly on both desktop and mobile screens.

- **Optional Enhancements** *(not implemented in basic version)*  
  - Dark mode toggle  
  - Animations for adding/deleting tasks  
  - Drag-and-drop reordering of todos

---
## Tech Stack

- **Frontend:** Vue 3 (`<script setup>`)  
- **Styling:** Tailwind CSS  
- **State Management & Persistence:** Vue `ref` + `computed` + `watch` + LocalStorage  
- **Animations (Optional):** `@vueuse/motion` or CSS transitions  

---

## Project Setup

Clone the repository and install dependencies:

```bash
git clone <your-repo-url>
cd todo-ase
npm install
Run development server: npm run dev 
Build for production: npm run build
