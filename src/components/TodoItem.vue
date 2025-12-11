<template>
  <li
    class="bg-white dark:bg-gray-800 px-4 py-3 md:px-5 md:py-4 rounded-lg shadow-sm
           flex items-start justify-between gap-4
           transition hover:shadow-md hover:scale-[1.01]"
    :class="todo.completed ? 'opacity-80' : ''"
  >
    <!-- Left: checkbox + content -->
    <div class="flex items-start gap-3 md:gap-4">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="$emit('toggle', todo.id)"
        class="mt-1 h-4 w-4 accent-sky-600 cursor-pointer"
      />

      <div>
        <!-- Title + badge -->
        <div class="flex flex-wrap gap-2 items-baseline">
          <h3
            :class="[
              'font-medium text-sm md:text-base',
              todo.completed
                ? 'line-through text-gray-500 dark:text-gray-400'
                : 'text-sky-700 dark:text-sky-300'
            ]"
          >
            {{ todo.title }}
          </h3>

          <span
            v-if="todo.dueDate"
            class="text-[11px] md:text-xs px-2 py-0.5 rounded-full text-white font-medium"
            :class="isOverdue ? 'bg-yellow-500' : 'bg-green-600'"
          >
            {{ formattedDue }}
          </span>
        </div>

        <!-- Description -->
        <p
          v-if="todo.description"
          class="text-sm text-gray-600 dark:text-gray-300 mt-1 leading-snug"
        >
          {{ todo.description }}
        </p>

        <!-- Meta -->
        <div class="text-[11px] text-gray-400 dark:text-gray-500 mt-1">
          Added: {{ new Date(todo.createdAt).toLocaleString() }}
        </div>
      </div>
    </div>

    <!-- Right: actions -->
    <div class="flex flex-col gap-2 ml-3 shrink-0">
      <button
        @click="$emit('edit', todo)"
        class="text-xs px-3 py-1.5 rounded-md border border-gray-300 dark:border-gray-500
               bg-white dark:bg-gray-700 text-gray-700 dark:text-gray-100
               hover:bg-sky-50 dark:hover:bg-sky-600 hover:text-sky-800 dark:hover:text-white
               focus:outline-none focus:ring-2 focus:ring-sky-300
               transition button-pop"
      >
        Edit
      </button>

      <button
        @click="$emit('delete', todo.id)"
        class="text-xs px-3 py-1.5 rounded-md bg-rose-600 dark:bg-rose-800
               text-white dark:text-white
               hover:bg-rose-200 dark:hover:bg-rose-600 hover:text-rose-800 dark:hover:text-white
               focus:outline-none focus:ring-2 focus:ring-rose-300
               transition button-pop"
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
