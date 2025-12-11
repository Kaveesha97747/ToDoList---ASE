<template>
  <div
    class="h-full lg:px-48  md:px-16 md:py-20 px-5 py-8 
           bg-gradient-to-b from-green-200 to-gray-300 
           dark:from-gray-700 dark:to-gray-300 
           text-gray-900 dark:text-gray-100"
    style="animation: fadeSlideIn 0.5s ease;"
  >
    <div class="w-full mx-auto">

      <!-- HEADER -->
      <header class="mb-6 flex flex-col gap-3 md:flex-row md:items-center md:justify-between">
  <div>
    <h1 class="text-2xl font-bold text-[black] dark:text-white">
      FocusPad – Emphasizes productivity and focus.
    </h1>
  </div>

  <!-- DARK MODE TOGGLE -->
  <div class="flex items-center gap-2 self-start md:self-auto">
    <span class="text-sm text-[#07232a] font-bold dark:text-gray-300">
      Theme
    </span>

    <button
      @click="toggleDarkMode"
      class="relative inline-flex items-center h-7 w-14 rounded-full
             bg-gray-200 dark:bg-gray-700 transition-colors duration-200
             focus:outline-none focus:ring-2 focus:ring-sky-300 button-pop"
    >
      <!-- icons -->
      <span
        class="absolute left-1 text-[10px]"
        :class="isDark ? 'opacity-0' : 'opacity-100'"
      >
        ☀️
      </span>
      <span
        class="absolute right-1 text-[10px]"
        :class="isDark ? 'opacity-100' : 'opacity-0'"
      >
        🌙
      </span>

      <!-- thumb -->
      <span
        class="inline-block h-5 w-5 rounded-full bg-white shadow
               transform transition-transform duration-200"
        :class="isDark ? 'translate-x-7' : 'translate-x-1'"
      ></span>
    </button>
  </div>
</header>


      <!-- FORM -->
      <section class="mb-4">
        <TodoForm
          :editingTodo="editingTodo"
          @save="handleSave"
          @cancelEdit="cancelEdit"
        />
      </section>

      <!-- FILTERS -->
      <section class="mb-4 flex gap-2 items-center">
        <div class="flex gap-1 bg-white dark:bg-gray-700 p-1 rounded shadow-sm">
          <button
            v-for="f in filters"
            :key="f"
            :class="[
              'px-3 py-1 rounded text-sm button-pop transition',
              filter === f 
                ? 'bg-sky-600 text-white hover:bg-sky-700' 
                : 'text-sky-700 dark:text-sky-300 hover:bg-gray-200 dark:hover:bg-gray-600'
            ]"
            @click="filter = f"
          >
            {{ f }}
          </button>
        </div>

        <div class="ml-auto text-sm text-gray-600 dark:text-gray-300">
          <span class="font-medium">{{ remaining }}</span> pending
        </div>
      </section>

      <!-- LIST -->
      <section>
        <TodoList
          :todos="filteredTodos"
          @toggle="toggleComplete"
          @edit="startEdit"
          @delete="deleteTodo"
        />
      </section>

      <!-- FOOTER -->
      <footer class="mt-10 text-center text-xs text-gray-400 dark:text-gray-500">
        Built with Vue 3 • LocalStorage
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

const STORAGE_KEY = 'ase_todos_v1'

// Load from LocalStorage
function loadTodos() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY)
    return raw ? JSON.parse(raw) : []
  } catch (e) {
    return []
  }
}

const todos = ref(loadTodos())

watch(todos, (n) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(n))
}, { deep: true })

const filters = ['All', 'Completed', 'Pending']
const filter = ref('All')

const filteredTodos = computed(() => {
  if (filter.value === 'All') return todos.value
  if (filter.value === 'Completed') return todos.value.filter(t => t.completed)
  return todos.value.filter(t => !t.completed)
})

const remaining = computed(() => todos.value.filter(t => !t.completed).length)

const editingTodo = ref(null)

function handleSave(payload) {
  if (payload.id) {
    const idx = todos.value.findIndex(t => t.id === payload.id)
    if (idx !== -1) {
      todos.value[idx] = { ...todos.value[idx], ...payload }
    }
  } else {
    const newTodo = {
      id: Date.now().toString(),
      title: payload.title,
      description: payload.description || '',
      dueDate: payload.dueDate || null,
      completed: false,
      createdAt: new Date().toISOString()
    }
    todos.value = [newTodo, ...todos.value]
  }
  editingTodo.value = null
}

function toggleComplete(id) {
  const t = todos.value.find(x => x.id === id)
  if (t) t.completed = !t.completed
}

function deleteTodo(id) {
  todos.value = todos.value.filter(t => t.id !== id)
}

function startEdit(todo) {
  editingTodo.value = { ...todo }
}

function cancelEdit() {
  editingTodo.value = null
}

/* DARK MODE LOGIC */
const isDark = ref(false)

onMounted(() => {
  const saved = localStorage.getItem('app_theme')
  if (saved === 'dark') {
    isDark.value = true
    document.documentElement.classList.add('dark')
  }
})

function toggleDarkMode() {
  isDark.value = !isDark.value
  const root = document.documentElement

  if (isDark.value) {
    root.classList.add('dark')
    localStorage.setItem('app_theme', 'dark')
  } else {
    root.classList.remove('dark')
    localStorage.setItem('app_theme', 'light')
  }
}
</script>
