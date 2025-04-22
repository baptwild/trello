<template>
  <div class="column">
    <ColumnTitle :modelValue="title" @update:modelValue="updateColumnTitle" />
    <DeleteButton @click="deleteColumn" />
    <button @click="addCard">+ Ajouter une carte</button>

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
import { defineComponent } from 'vue';
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
  methods: {
    updateColumnTitle(newTitle: string) {
      this.$emit('update-column-title', { status: this.status, newTitle });
    },
    addCard() {
      this.$emit('add-card', this.status);
    },
    removeCard(card: any) {
      this.$emit('remove-card', { card });
    },
    updateCardTitle({ card, newTitle }: any) {
      card.title = newTitle;
    },
    onDrag() {
      // Mise à jour des statuts dans cette colonne après drop
      this.cards.forEach(card => {
        card.status = this.status;
      });
      this.$emit('update-cards', this.status, [...this.cards]);
    },
    deleteColumn() {
      this.$emit('remove-column', this.status);
    },
  },
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

button {
  margin-top: 12px;
  padding: 8px 12px;
  background-color: #3f83f8;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #2563eb;
}
.delete-btn:hover {
  background-color: #f87171;
}
</style>
