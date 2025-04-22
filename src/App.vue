<template>
  <div id="app">
    <!-- Utilisation du composant Board qui contient les colonnes et cartes -->
    <Board />
  </div>
</template>

<script>
// Importation du composant Board
import Board from './components/templates/Board.vue';

export default {
  components: {
    Board // Déclaration du composant Board
  }
};
</script>

<style>
/* Si tu as des styles globaux, tu peux les ajouter ici */
body {
  font-family: Arial, sans-serif;
}
</style>
