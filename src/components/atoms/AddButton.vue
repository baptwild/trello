<template>
  <button class="add-btn" @click="$emit('click')">➕</button>
</template>

<script setup>
defineEmits(['click']);
</script>

<style scoped>
.add-btn {
  background-color: transparent;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: green;
}

.add-btn:hover {
  color: darkgreen;
}
</style>
