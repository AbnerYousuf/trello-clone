<template>
  <div v-if="isOpen" @keydown.esc="emit('close')" tabindex="0">
    <div
      class="fixed inset-0 flex items-center justify-center backdrop-blur-xs"
      role="dialog"
      aria-modal="true"
    >
      <div class="bg-white rounded-lg shadow-lg p-5 w-full max-w-md">
        <h2 class="text-xl font-semibold mb-4" aria-label="Add New Card">Add New Card</h2>
        <input
          type="text"
          class="mb-4 w-full p-2 border rounded-lg"
          aria-label="Card Title"
          placeholder="Card Title"
          ref="titleInput"
        />
        <textarea
          class="mb-4 w-full p-2 border rounded-lg"
          aria-label="Card Description"
          placeholder="Card Description"
          ref="descriptionInput"
        ></textarea>
        <div class="flex justify-end gap-2">
          <button
            class="bg-gray-200 text-black px-4 py-2 rounded hover:bg-gray-300 cursor-pointer"
            @click="emit('close')"
          >
            Cancel
          </button>
          <button
            class="bg-teal-500 text-white px-4 py-2 rounded hover:bg-teal-600 cursor-pointer"
            @click="emit('close')"
          >
            Save
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

const props = defineProps<{
  isOpen: boolean
}>()

const emit = defineEmits<{
  (e: 'close'): void
}>()

const titleInput = ref<HTMLInputElement | null>(null)
const descriptionInput = ref<HTMLTextAreaElement | null>(null)

watch(
  () => props.isOpen,
  (isOpen) => {
    if (isOpen) {
      // Focus the title input when the dialog opens
      setTimeout(() => {
        titleInput.value?.focus()
      }, 0)
    }
  },
)
</script>
