<template>
  <div class="column">
    <ColumnTitle :modelValue="title" @update:modelValue="updateColumnTitle" />

    <!-- Bouton de suppression de la colonne -->
    <DeleteButton @click="deleteColumn" />

    <button @click="addCard">+ Ajouter une carte</button>

    <draggable
        v-model="localCards"
        group="cards"
        item-key="id"
        class="card-list"
        @end="onDragEnd"
    >
      <template #item="{ element }">
        <Card :card="element" @update-title="updateCardTitle" @delete-card="removeCard" />
      </template>
    </draggable>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ColumnTitle from '../atoms/ColumnTitle.vue'; // Import du nouveau composant
import DeleteButton from '../atoms/DeleteButton.vue'; // Import du bouton delete
import Card from '../molecules/Card.vue';
import draggable from 'vuedraggable';

export default defineComponent({
  name: 'ColumnList',
  components: {
    ColumnTitle, // Ajout du composant ColumnTitle
    DeleteButton, // Ajout du composant DeleteButton
    Card,
    draggable,
  },
  props: {
    title: String,
    status: String,
    cards: Array,
  },
  emits: ['update-column-title', 'add-card', 'remove-card', 'update-cards', 'remove-column'],
  data() {
    return {
      localCards: [...this.cards],
    };
  },
  watch: {
    cards(newCards) {
      this.localCards = [...newCards];
    },
  },
  methods: {
    updateColumnTitle(newTitle: string) {
      this.$emit('update-column-title', { status: this.status, newTitle });
    },
    addCard() {
      this.$emit('add-card', this.status);
    },
    removeCard(card: any) {
      this.$emit('remove-card', { status: this.status, card });
    },
    updateCardTitle({ card, newTitle }: any) {
      card.title = newTitle;
    },
    onDragEnd() {
      // Maintien du status intact lors du déplacement des cartes
      this.localCards.forEach(card => {
        card.status = this.status; // Réassurer que le status de chaque carte est correct
      });

      // Informer le parent avec les cartes mises à jour
      this.$emit('update-cards', this.status, [...this.localCards]);
    },
    deleteColumn() {
      this.$emit('remove-column', this.status); // Notifie le parent pour supprimer la colonne
    },
  },
});
</script>

<style scoped>
.column {
  width: 300px;
  padding: 10px;
  border-radius: 8px;
  background-color: #f4f5f7;
  margin-right: 20px;
}

.card-list {
  margin-top: 10px;
}
</style>
