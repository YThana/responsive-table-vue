<script setup lang="ts">
import { ref } from 'vue';

const showSidebar = ref<boolean>(false);
</script>

<template>
  <div class="flex max-w-screen">
    <aside
      class="fixed lg:sticky z-50 top-0 h-screen bg-none shrink-0 py-4 pl-4 w-64 transition-all duration-200"
      :class="showSidebar ? 'translate-x-0' : '-translate-x-[17rem] lg:translate-x-0'"
    >
      <button
        @click="showSidebar = !showSidebar"
        class="sm-button absolute top-8 right-4 flex lg:hidden"
      >
        close
      </button>

      <div
        class="h-full overflow-auto shadow lg:shadow-none shadow-indigo-50 p-4 bg-indigo-50 ring-inset ring-1 ring-indigo-100 rounded-2xl"
      >
        <slot name="sidebar" />
      </div>
    </aside>

    <main class="flex-1 p-4 overflow-auto">
      <button
        v-if="!showSidebar"
        class="sm-button fixed right-4 top-4 flex lg:hidden"
        @click="showSidebar = !showSidebar"
      >
        Open
      </button>
      <slot name="main"></slot>
    </main>
  </div>
</template>

<style scoped></style>
