<template>
  <div class="card-status">
    <select v-model="selectedStatus" @change="updateStatus">
      <option value="To Do">To Do</option>
      <option value="Doing">Doing</option>
      <option value="To Review">To Review</option>
      <option value="Done">Done</option>
    </select>
  </div>
</template>

<script lang="ts" setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps<{ modelValue: string }>()
const emit = defineEmits(['update:modelValue'])

const selectedStatus = props.modelValue

const updateStatus = () => {
  emit('update:modelValue', selectedStatus)
}
</script>

<style scoped>
.card-status select {
  width: 100%;
  padding: 5px;
  font-size: 1rem;
  border-radius: 4px;
}
</style>
