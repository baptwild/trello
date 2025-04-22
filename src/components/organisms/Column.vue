<template>
  <div class="column">
    <ColumnTitle :modelValue="title" @update:modelValue="updateColumnTitle" />
    <DeleteButton @click="deleteColumn" />

    <form class="add-card-form" @submit.prevent="submitCard">
      <input
          type="text"
          v-model="newCardTitle"
          placeholder="Titre de la carte"
          required
      />
      <textarea
          v-model="newCardDescription"
          placeholder="Description"
          rows="2"
      />
      <button type="submit">+ Ajouter une carte</button>
    </form>

    <draggable
        :list="cards"
        group="cards"
        item-key="id"
        class="card-list"
        @change="onDrag"
    >
      <template #item="{ element }">
        <Card
            :card="element"
            @update-title="updateCardTitle"
            @delete-card="() => removeCard(element)"
        />
      </template>
    </draggable>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import ColumnTitle from '../atoms/ColumnTitle.vue';
import DeleteButton from '../atoms/DeleteButton.vue';
import Card from '../molecules/Card.vue';
import draggable from 'vuedraggable';

export default defineComponent({
  name: 'ColumnList',
  components: {
    ColumnTitle,
    DeleteButton,
    Card,
    draggable,
  },
  props: {
    title: String,
    status: String,
    cards: {
      type: Array,
      required: true
    }
  },
  emits: ['update-column-title', 'add-card', 'remove-card', 'update-cards', 'remove-column'],
  setup(props, { emit }) {
    const newCardTitle = ref('');
    const newCardDescription = ref('');

    const submitCard = () => {
      if (!newCardTitle.value.trim()) return;

      emit('add-card', {
        title: newCardTitle.value,
        description: newCardDescription.value,
        status: props.status
      });

      newCardTitle.value = '';
      newCardDescription.value = '';
    };

    const updateColumnTitle = (newTitle: string) => {
      emit('update-column-title', { status: props.status, newTitle });
    };

    const removeCard = (card: any) => {
      emit('remove-card', { card });
    };

    const updateCardTitle = ({ card, newTitle }: any) => {
      card.title = newTitle;
    };

    const onDrag = () => {
      props.cards.forEach(card => {
        card.status = props.status;
      });
      emit('update-cards', props.status, [...props.cards]);
    };

    const deleteColumn = () => {
      emit('remove-column', props.status);
    };

    return {
      newCardTitle,
      newCardDescription,
      submitCard,
      updateColumnTitle,
      removeCard,
      updateCardTitle,
      onDrag,
      deleteColumn
    };
  }
});
</script>

<style scoped>
.column {
  width: 360px;
  min-height: 100px;
  padding: 16px;
  border-radius: 16px;
  background-color: #ffffff;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.2s ease-in-out;
  display: flex;
  flex-direction: column;
}

.column:hover {
  box-shadow: 0 12px 20px rgba(0, 0, 0, 0.08);
}

.card-list {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex-grow: 1;
}

.add-card-form {
  margin-top: 12px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.add-card-form input,
.add-card-form textarea {
  padding: 8px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 14px;
  resize: none;
}

.add-card-form button {
  padding: 8px 12px;
  background-color: #3f83f8;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  transition: background-color 0.2s;
}

.add-card-form button:hover {
  background-color: #2563eb;
}
</style>
