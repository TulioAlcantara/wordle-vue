<template>
  <div
    class="border-2 border-gray-300 border-solid uppercase flex items-center justify-center h-16 text-3xl transition-all duration-300"
    :class="{
      'bg-green-500 border-green-500 text-white': color == 'green',
      'bg-yellow-500 border-yellow-500 text-white': color == 'yellow',
      'bg-gray-500 border-gray-500 text-white': color == 'gray',
    }"
  >
    {{ letter }}
  </div>
</template>

<script setup lang="ts">
defineProps({
  letter: {
    type: String,
    default: "",
  },
  color: {
    type: String,
    default: "",
  },
});
</script>

<style scoped></style>
