<template>
  <div class="card-title">
    <input
        type="text"
        :value="modelValue"
        @input="onInput"
        placeholder="Titre de la carte"
    />
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
.card-title {
  font-size: 1.2rem;
  font-weight: bold;
}

input {
  width: 100%;
  font-size: 1.1rem;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}
</style>
