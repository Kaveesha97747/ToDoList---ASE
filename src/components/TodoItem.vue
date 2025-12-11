<template>
  <li
    class="bg-white dark:bg-gray-800 p-3 rounded shadow-sm flex items-start justify-between 
           transition hover:shadow-md hover:scale-[1.01]"
    :class="todo.completed ? 'opacity-80' : ''"
  >
    <div class="flex items-start gap-3">
      <input 
        type="checkbox" 
        :checked="todo.completed" 
        @change="$emit('toggle', todo.id)"
        class="mt-1"
      />

      <div>
        <div class="flex gap-2 items-baseline">
          <h3 :class="[
            'font-medium',
            todo.completed 
              ? 'line-through text-gray-500' 
              : 'text-sky-700 dark:text-sky-300'
          ]">
            {{ todo.title }}
          </h3>

          <span 
            v-if="todo.dueDate"
            class="text-xs px-2 py-0.5 rounded text-white"
            :class="isOverdue ? 'bg-yellow-500' : 'bg-green-600'"
          >
            {{ formattedDue }}
          </span>
        </div>

        <p v-if="todo.description" class="text-sm text-gray-600 dark:text-gray-300 mt-1">
          {{ todo.description }}
        </p>

        <div class="text-xs text-gray-400 dark:text-gray-500 mt-1">
          Added: {{ new Date(todo.createdAt).toLocaleString() }}
        </div>
      </div>
    </div>

    <div class="flex flex-col gap-2">
  <button 
    @click="$emit('edit', todo)" 
    class="text-xs px-2 py-1 rounded border dark:border-gray-500 button-pop
           hover:bg-sky-100 dark:hover:bg-sky-600 hover:text-sky-800 dark:hover:text-white transition"
  >
    Edit
  </button>
  <button 
    @click="$emit('delete', todo.id)" 
    class="text-xs px-2 py-1 rounded bg-rose-100 dark:bg-rose-400 
           dark:text-white text-rose-700 button-pop
           hover:bg-rose-200 dark:hover:bg-rose-600 hover:text-rose-800 dark:hover:text-white transition"
  >
    Delete
  </button>
</div>

  </li>
</template>

<script setup>
import { ref, computed } from 'vue' 

const props = defineProps({
  todo: { type: Object, required: true }
})

const formattedDue = computed(() => {
  if (!props.todo.dueDate) return ''
  // format YYYY-MM-DD -> friendly
  try {
    const d = new Date(props.todo.dueDate + 'T00:00:00')
    return d.toLocaleDateString()
  } catch {
    return props.todo.dueDate
  }
})

const isOverdue = computed(() => {
  if (!props.todo.dueDate) return false
  const today = new Date()
  const due = new Date(props.todo.dueDate + 'T23:59:59')
  return !props.todo.completed && due < today
})
</script>
