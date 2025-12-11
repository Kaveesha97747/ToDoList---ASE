<template>
  <form
    class="bg-white dark:bg-gray-800 px-4 py-3 md:px-6 md:py-4 rounded-lg shadow-sm transition"
    @submit.prevent="save"
  >
    <div class="flex flex-col gap-4">

      <!-- Title -->
      <input
        v-model="title"
        type="text"
        placeholder="Title (required)"
        class="border border-gray-300 dark:border-gray-600 dark:bg-gray-700 dark:text-white
               rounded-md px-3 py-2 text-sm
               focus:outline-none focus:ring-2 focus:ring-sky-300 focus:border-sky-400"
        required
      />

      <!-- Description -->
      <textarea
        v-model="description"
        rows="3"
        placeholder="Description (optional)"
        class="border border-gray-300 dark:border-gray-600 dark:bg-gray-700 dark:text-white
               rounded-md px-3 py-2 text-sm
               focus:outline-none focus:ring-2 focus:ring-sky-300 focus:border-sky-400 resize-none"
      ></textarea>

      <!-- Due date -->
      <div class="flex flex-wrap items-center gap-2">
        <input
          v-model="dueDate"
          type="date"
          class="border border-gray-300 dark:border-gray-600 dark:bg-gray-700 dark:text-white
                 rounded-md px-3 py-2 text-sm focus:outline-none focus:ring-1 focus:ring-sky-300"
        />
        <span class="text-sm text-gray-500 dark:text-gray-300">Due date</span>
      </div>

      <!-- Actions -->
      <div class="flex flex-wrap gap-2 pt-1">
        <button
          type="submit"
          class="px-4 py-2 rounded-md bg-sky-600 text-white text-sm font-medium
                 hover:bg-sky-700 active:bg-sky-800
                 focus:outline-none focus:ring-2 focus:ring-sky-300
                 transition button-pop"
        >
          {{ isEditing ? 'Update' : 'Add' }}
        </button>

        <button
          v-if="isEditing"
          type="button"
          @click="cancel"
          class="px-4 py-2 rounded-md border border-gray-300 dark:border-gray-500
                 text-sm text-gray-700 dark:text-gray-200 bg-white dark:bg-gray-700
                 hover:bg-gray-100 dark:hover:bg-gray-600
                 focus:outline-none focus:ring-2 focus:ring-gray-300
                 transition button-pop"
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
