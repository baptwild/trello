<template>
  <div class="card-description">
    <p v-if="!isEditing" @click="edit">{{ modelValue || 'Ajouter une description...' }}</p>
    <textarea
        v-else
        v-model="editable"
        @blur="save"
        @keyup.enter="save"
        ref="input"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref, watch, onMounted, defineProps, defineEmits } from 'vue'

const props = defineProps<{ modelValue: string }>()
const emit = defineEmits(['update:modelValue'])

const isEditing = ref(false)
const editable = ref(props.modelValue)
const input = ref<HTMLTextAreaElement | null>(null)

watch(() => props.modelValue, (newVal) => {
  editable.value = newVal
})

const edit = () => {
  isEditing.value = true
  nextTick(() => input.value?.focus())
}

const save = () => {
  emit('update:modelValue', editable.value)
  isEditing.value = false
}
</script>

<style scoped>
.card-description p {
  cursor: pointer;
  color: #444;
  margin: 0;
}

textarea {
  width: 100%;
  resize: vertical;
  min-height: 60px;
  padding: 5px;
  font-family: inherit;
}
</style>
