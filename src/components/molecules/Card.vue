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
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: relative;
}

.card-header {
  font-weight: bold;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.delete-wrapper {
  margin-top: 5px;
  align-self: flex-end;
}

.card-body {
  margin-top: 10px;
}

.card-body div {
  margin-bottom: 10px;
}
</style>
