<template>
  <button class="delete-btn" @click="$emit('delete')">🗑</button>
</template>

<script setup>
defineEmits(['delete']);
</script>

<style scoped>
.delete-btn {
  background: transparent;
  border: none;
  cursor: pointer;
  font-size: 1.2rem;
  color: white;
  transition: color 0.2s ease-in-out;
}

.delete-btn:hover {
  color: red;
}
</style>
