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
          @update-title="updateColumnTitle"
          @add-card="addCard"
          @remove-column="removeColumn"
      @remove-card="removeCard"
      @updateCards="updateCards"
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
        title: "Nouvelle carte",
        description: "",
        deadline: "2025-12-31",
        status: status
      })
    },
    removeCard({ status, card }: { status: string, card: any }) {
      this.cards = this.cards.filter(c => !(c.status === status && c.title === card.title))
    },
    updateCards(status: string, updatedCards: any[]) {
      this.cards = this.cards.filter(card => card.status !== status);
      updatedCards.forEach(card => {
        this.cards.push(card);
      });
    }
  }
})
</script>

<style scoped>
.board-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 20px;
}

.board {
  display: flex;
  gap: 20px;
  width: 100%;
  overflow-x: auto;
  padding-top: 10px;
}
</style>
