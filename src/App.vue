<template>
  <main class="p-5 font-sans">
    <div class="flex gap-5 py-5 overflow-x-auto">
      <div
        class="bg-teal-200 rounded-lg p-3 min-w-62.5 flex flex-col"
        v-for="list in lists"
        :key="list.id"
      >
        <h2 class="font-medium mb-2 text-teal-800">{{ list.title }}</h2>
        <div
          class="bg-teal-100 p-2 my-2 rounded shadow cursor-pointer"
          v-for="card in list.cards"
          :key="card.id"
        >
          <span class="text-sm font-medium text-teal-800">{{ card.title }}</span>
          <p class="text-xs text-teal-600">{{ card.description }}</p>
        </div>
      <!-- we do it this way because a nested for loop is necessary since it's a nested data structure-->
        <button
          class="cursor-pointer rounded w-full bg-transparent text-teal-800 hover:bg-teal-100 p-2 text-left mt-2 text-sm font-medium"
        >
          + Add Card
        </button>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
//im using reactive here because this is a complex data structure
//and i wont reassign this so i dont need the reactivity of ref
//the course said this was more performance effective but didnt go into detail
//so i looked it up myself :D

interface Card {
  id: number
  title: string
  description: string
}

interface List {
  id: number
  title: string
  cards: Card[]
}

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
</script>
