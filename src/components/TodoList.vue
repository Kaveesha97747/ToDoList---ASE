<template>
  <TransitionGroup
    tag="ul"
    name="todo"
    class="space-y-3 md:space-y-4"
  >
    <!-- Empty state -->
    <li
      v-if="todos.length === 0"
      key="empty"
      class="text-center text-sm text-gray-400 dark:text-white
             py-6 border border-dashed border-gray-200 dark:border-gray-700
             rounded-lg bg-white/40 dark:bg-gray-800/40"
    >
      No todos yet. Add your first task to get started.
    </li>

    <!-- Items -->
    <TodoItem
      v-for="t in todos"
      :key="t.id"
      :todo="t"
      @toggle="$emit('toggle', $event)"
      @edit="$emit('edit', $event)"
      @delete="$emit('delete', $event)"
    />
  </TransitionGroup>
</template>

<script setup>
import TodoItem from './TodoItem.vue'

defineProps({
  todos: {
    type: Array,
    default: () => []
  }
})
</script>
