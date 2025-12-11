# Todo List

A simple, responsive Todo List application built using **Vue 3**, **Tailwind CSS**, and **ES6**.  
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
<img width="1853" height="880" alt="light-05" src="https://github.com/user-attachments/assets/79b9f2ee-18c4-4679-b256-13fb606f1b27" />
<img width="385" height="788" alt="light-m-01" src="https://github.com/user-attachments/assets/622666a5-f5d3-4515-b081-48613a7e021c" />

<img width="1852" height="793" alt="light-03" src="https://github.com/user-attachments/assets/2c9fcb31-5904-431f-a926-5443b0e14459" />
<img width="1831" height="880" alt="dark-01" src="https://github.com/user-attachments/assets/b048e768-b603-4f2d-b25c-53a578e93ae9" />
<img width="435" height="788" alt="dark-02" src="https://github.com/user-attachments/assets/08e9494a-9d4c-4b54-bd26-4b7ef7ee3f78" />
