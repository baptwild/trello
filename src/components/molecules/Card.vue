<template>
  <div class="card">
    <div class="card-header">
      <CardTitle v-model="card.title" />
      <div class="delete-wrapper">
        <DeleteButton @delete="deleteCard" />
      </div>
    </div>

    <div class="card-body">
      <CardDescription v-model="card.description" />
      <CardDeadline v-model="card.deadline" />
      <CardStatus v-model="card.status" />
    </div>
  </div>
</template>

<script lang="ts" setup>
import { defineProps, defineEmits } from 'vue'
import CardTitle from '../atoms/CardTitle.vue'
import CardDescription from '../atoms/CardDescription.vue'
import CardDeadline from '../atoms/CardDeadline.vue'
import CardStatus from '../atoms/CardStatus.vue'
import DeleteButton from '../atoms/DeleteButton.vue'

const props = defineProps<{ card: { title: string, description: string, deadline: string, status: string } }>()
const emit = defineEmits(['update:card', 'delete-card'])

const updateCard = (field: string, value: string) => {
  emit('update:card', { ...props.card, [field]: value })
}

const deleteCard = () => {
  emit('delete-card', props.card)
}
</script>

<style scoped>
.card {
  background-color: #f9fafb;
  border-left: 5px solid #6a6c71;
  border-radius: 16px;
  padding: 16px;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  position: relative;
}

.card:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 24px rgba(0, 0, 0, 0.12);
}

.card-header {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 6px;
  margin-bottom: 12px;
}

.delete-wrapper {
  align-self: flex-end;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px;
  height: 32px;
  width: 32px;
  border-radius: 8px;
  transition: background-color 0.2s ease;
  cursor: pointer;
}

.delete-wrapper:hover {
  background-color: #f87171;
}

.delete-wrapper :deep(svg),
.delete-wrapper ::v-deep svg {
  fill: white;
  width: 16px;
  height: 16px;
}

.card-body {
  display: flex;
  flex-direction: column;
  gap: 10px;
  font-size: 14px;
}
</style>
