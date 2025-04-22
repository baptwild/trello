<template>
  <div class="board-container">
    <BoardTitle title="Tableau de Tâches" />
    <div class="board">
      <ColumnList
          v-for="(column, index) in columnList"
          :key="index"
          :title="column.title"
          :status="column.status"
          :cards="getCardsForColumn(column.status)"
          @update-column-title="updateColumnTitle"
          @add-card="addCard"
          @remove-column="removeColumn"
          @remove-card="removeCard"
          @update-cards="updateCards"
      />
      <AddButton @click="addColumn" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import BoardTitle from '../molecules/BoardTitle.vue'
import ColumnList from '../organisms/Column.vue'
import AddButton from '../atoms/AddButton.vue'

export default defineComponent({
  components: {
    BoardTitle,
    ColumnList,
    AddButton,
  },
  data() {
    return {
      columnList: [
        { title: "To Do", status: "To Do" },
        { title: "Doing", status: "Doing" },
        { title: "To Review", status: "To Review" },
        { title: "Done", status: "Done" }
      ],
      cards: []
    }
  },
  methods: {
    getCardsForColumn(status: string) {
      return this.cards.filter(card => card.status === status)
    },
    updateColumnTitle({ status, newTitle }: { status: string, newTitle: string }) {
      const column = this.columnList.find(col => col.status === status)
      if (column) {
        column.title = newTitle
      }
    },
    addColumn() {
      this.columnList.push({
        title: "Nouvelle colonne",
        status: "New_" + Date.now()
      })
    },
    removeColumn(status: string) {
      this.columnList = this.columnList.filter(col => col.status !== status)
      this.cards = this.cards.filter(card => card.status !== status)
    },
    addCard(status: string) {
      this.cards.push({
        id: Date.now() + Math.random(),
        title: "Nouvelle carte",
        description: "",
        deadline: "2025-12-31",
        status: status
      });
    },
    removeCard({ card }: { card: any }) {
      this.cards = this.cards.filter(c => c.id !== card.id)
    },
    updateCards(status: string, updatedCards: any[]) {
      // Supprimer toutes les anciennes cartes de cette colonne
      this.cards = this.cards.filter(card => card.status !== status)

      // Ajouter les nouvelles cartes avec le bon statut
      updatedCards.forEach(card => {
        card.status = status
        this.cards.push(card)
      })
    }
  }
})
</script>

<style scoped>
.board-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px;
  min-height: 100vh;
  background: linear-gradient(145deg, #e8f0ff, #fdfdfe);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.board {
  display: flex;
  gap: 16px;
  width: 100%;
  overflow-x: auto;
  padding: 20px 0;
  justify-content: flex-start;
}

.board-title {
  text-align: center;
  font-size: 2rem;
  font-weight: 700;
  color: #1e3a8a;
  margin-bottom: 20px;
  background: #e0e7ff;
  padding: 12px 24px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(30, 58, 138, 0.1);
}
</style>
