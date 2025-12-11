<template>
  <form class="bg-white dark:bg-gray-800 p-4 rounded shadow-sm transition">
    <div class="flex flex-col gap-3">

      <input
        v-model="title"
        type="text"
        placeholder="Title (required)"
        class="border dark:border-gray-600 dark:bg-gray-700 dark:text-white 
               rounded px-3 py-2 focus:outline-none focus:ring-2 
               focus:ring-sky-200"
        required
      />

      <textarea
        v-model="description"
        rows="3"
        placeholder="Description (optional)"
        class="border dark:border-gray-600 dark:bg-gray-700 dark:text-white 
               rounded px-3 py-2 focus:outline-none focus:ring-2 
               focus:ring-sky-200"
      ></textarea>

      <div class="flex gap-2 items-center">
        <input 
          v-model="dueDate" 
          type="date" 
          class="border dark:border-gray-600 dark:bg-gray-700 dark:text-white 
                 rounded px-3 py-2" 
        />
        <div class="text-sm text-gray-500 dark:text-gray-300">Due date</div>
      </div>

      <div class="flex gap-2">
        <button
  type="submit"
  class="px-4 py-2 rounded bg-sky-600 text-white text-sm 
         hover:bg-sky-700 transition button-pop"
>
  {{ isEditing ? 'Update' : 'Add' }}
</button>


        <button 
          v-if="isEditing" 
          type="button" 
          @click="cancel" 
          class="px-3 py-2 rounded border dark:border-gray-500 
                 text-sm button-pop"
        >
          Cancel
        </button>
      </div>

    </div>
  </form>
</template>


<script setup>
import { ref, watch, computed, defineProps, defineEmits } from 'vue'

const props = defineProps({
  editingTodo: { type: Object, default: null }
})
const emit = defineEmits(['save', 'cancelEdit'])

const title = ref('')
const description = ref('')
const dueDate = ref(null)
const currentId = ref(null)

const isEditing = computed(() => !!currentId.value)

watch(
  () => props.editingTodo,
  (v) => {
    if (v) {
      title.value = v.title || ''
      description.value = v.description || ''
      dueDate.value = v.dueDate || null
      currentId.value = v.id
    } else {
      title.value = ''
      description.value = ''
      dueDate.value = null
      currentId.value = null
    }
  },
  { immediate: true }
)

function save() {
  if (!title.value.trim()) return
  emit('save', {
    id: currentId.value,
    title: title.value.trim(),
    description: description.value.trim(),
    dueDate: dueDate.value || null
  })
  // reset only when adding new
  if (!currentId.value) {
    title.value = ''
    description.value = ''
    dueDate.value = null
  }
}

function cancel() {
  emit('cancelEdit')
}
</script>
