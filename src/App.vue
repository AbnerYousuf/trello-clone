<template>
  <main class="p-5 font-sans">
    <div class="flex gap-5 py-5 overflow-x-auto">
      <div class="bg-teal-200 rounded-lg p-3 min-w-62.5 flex flex-col">
        <h2 class="font-medium mb-2 text-teal-800">list</h2>
        <div class="bg-teal-100 p-2 my-2 rounded shadow cursor-pointer">
          <span class="text-sm font-medium text-teal-800">title</span>
          <p class="text-xs text-teal-600">description</p>
        </div>
        <button class="cursor-pointer rounded w-full bg-transparent text-teal-800 hover:bg-teal-100 p-2 text-left mt-2 text-sm font-medium">
          + Add Card
        </button>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts"></script>
