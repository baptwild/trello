<template>
  <div class="card-deadline">
    <label>
      Deadline :
      <input type="date" :value="modelValue" @input="onInput" />
    </label>
  </div>
</template>

<script lang="ts" setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps<{ modelValue: string }>()
const emit = defineEmits(['update:modelValue'])

const onInput = (event: Event) => {
  const target = event.target as HTMLInputElement
  emit('update:modelValue', target.value)
}
</script>

<style scoped>
.card-deadline {
  margin-top: 10px;
  color: #333;
}

input[type="date"] {
  padding: 4px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
