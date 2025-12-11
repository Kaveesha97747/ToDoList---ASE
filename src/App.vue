<template>
  <div class="h-screen md:px-48 md:py-20 px-5 py-8 
              bg-gradient-to-b from-sky-50 to-white 
              dark:from-gray-900 dark:to-gray-800 
              text-gray-900 dark:text-gray-100"
       style="animation: fadeSlideIn 0.5s ease;">
    
    <div class="w-full mx-auto">

      <header class="mb-6 flex items-center justify-between">
        <div>
          <h1 class="text-2xl font-bold text-sky-700 dark:text-sky-300">
            Todo — ASE Task
          </h1>
          <p class="text-sm text-gray-500 dark:text-gray-400">
            Mobile first • LocalStorage
          </p>
        </div>

        <!-- DARK MODE TOGGLE -->
        <button @click="toggleDarkMode"
          class="px-3 py-1 text-sm rounded bg-gray-200 dark:bg-gray-700 
                 dark:text-white hover:bg-gray-300 dark:hover:bg-gray-600 
       transition button-pop">
          {{ isDark ? 'Light' : 'Dark' }}
        </button>
      </header>

      <section class="mb-4">
        <TodoForm
          :editingTodo="editingTodo"
          @save="handleSave"
          @cancelEdit="cancelEdit"
        />
      </section>

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

      <section>
        <TodoList
          :todos="filteredTodos"
          @toggle="toggleComplete"
          @edit="startEdit"
          @delete="deleteTodo"
        />
      </section>

      <footer class="mt-8 text-center text-xs text-gray-400 dark:text-gray-500">
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
