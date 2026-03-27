<template>
  <main class="p-5 font-sans">
    <div class="flex gap-5 py-5 overflow-x-auto">
      <div
        class="bg-teal-200 rounded-lg p-3 min-w-62.5 flex flex-col"
        v-for="(list, listIndex) in lists"
        :key="list.id"
      >
        <h2 class="font-medium mb-2 text-teal-800">{{ list.title }}</h2>
        <!-- we do it this way because a nested for loop is necessary since it's a nested data structure-->
        <Draggable :list="list.cards" group="cards" item-key="id">
          <template #item="{ element }">
            <div
              class="bg-teal-100 p-2 my-2 rounded shadow flex justify-between items-center group"
            >
              <div class="flex-1 cursor-pointer" @click="openDialog(listIndex, element)">
                <span class="text-sm font-medium text-teal-800">{{ element.title }}</span>
                <p class="text-xs text-teal-600">{{ element.description }}</p>
              </div>
              <button
                @click="deleteCard(listIndex, element.id)"
                class="ml-2 text-teal-600 hover:text-red-500 opacity-0 group-hover:opacity-100 transition-opacity"
              >
                ×
              </button>
            </div>
          </template>
        </Draggable>
        <button
          class="cursor-pointer rounded w-full bg-transparent text-teal-800 hover:bg-teal-100 p-2 text-left mt-2 text-sm font-medium"
          @click="openDialog(listIndex)"
        >
          + Add Card
        </button>
      </div>
      <DialogComponent
        :isOpen="isDialogOpen"
        :card="editingCard"
        :mode="dialogMode"
        @close="closeDialog"
        @save="saveCard"
      />
    </div>
  </main>
</template>

<script setup lang="ts">
import DialogComponent from '@/components/DialogComponent.vue'
import type { List, Card } from '@/types'
import Draggable from 'vuedraggable'
import { computed, reactive, ref } from 'vue'
//im using reactive here because this is a complex data structure
//and i wont reassign this so i dont need ref's reassignability
//the course said this was more performance effective but didnt go into detail
//so i looked it up myself :D

const lists = reactive<List[]>([
  {
    id: 1,
    title: 'To Do',
    cards: [
      { id: 1, title: 'Task 1', description: 'Description for Task 1' },
      { id: 2, title: 'Task 2', description: 'Description for Task 2' },
    ],
  },
  {
    id: 2,
    title: 'In Progress',
    cards: [
      { id: 3, title: 'Task 3', description: 'Description for Task 3' },
      { id: 4, title: 'Task 4', description: 'Description for Task 4' },
    ],
  },
  {
    id: 3,
    title: 'Done',
    cards: [{ id: 5, title: 'Task 5', description: 'Description for Task 5' }],
  },
])

const isDialogOpen = ref(false)

const openDialog = (listId: number, card?: Card) => {
  isDialogOpen.value = true
  editingListId.value = listId
  editingCard.value = card === undefined ? null : card
}

const closeDialog = () => {
  isDialogOpen.value = false
  editingCard.value = null
  editingListId.value = null
}

const editingCard = ref<Card | null>(null)
const editingListId = ref<number | null>(null)
const dialogMode = computed(() => (editingCard.value === null ? 'add' : 'edit'))

const saveCard = (card: Card) => {
  if (editingListId.value === null) {
    return
  }
  const list = lists[editingListId.value]
  if (!list) return

  if (dialogMode.value === 'add') {
    //adding
    const newId = Math.max(...lists.flatMap((list) => list.cards.map((c) => c.id)))
    list.cards.push({ ...card, id: newId })
  } else {
    //editing
    const cardIndex = list.cards.findIndex((c) => c.id === card.id)
    if (cardIndex !== -1) {
      list.cards[cardIndex] = card
    }
  }
  closeDialog()
}

const deleteCard = (listIndex: number, cardId: number) => {
  const list = lists[listIndex]
  if (!list) return
  const cardIndex = list.cards.findIndex((c) => c.id === cardId)
  if (cardIndex !== -1) {
    list.cards.splice(cardIndex, 1)
  }
}
</script>
