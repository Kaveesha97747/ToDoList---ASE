<template>
  <TransitionGroup 
    tag="ul" 
    name="todo" 
    class="space-y-3"
  >
    <li 
      v-if="todos.length === 0" 
      key="empty" 
      class="text-center text-gray-400 dark:text-gray-500"
    >
      No todos yet
    </li>

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
  todos: Array
})
</script>
