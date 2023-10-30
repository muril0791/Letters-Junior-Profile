<template>
  <div class="board">
    <!-- ... restante do template -->
    <div
      v-for="(cell, index) in cells"
      :key="index"
      class="cell"
      :class="{ bonus: cell.bonus }"
      @click="selectCell(index)"
    >
      <Pawn v-if="cell.pawn" :color="cell.pawn.color" :playerName="cell.pawn.playerName" />
      <div class="cell-label" v-if="cell.bonus">?</div>
    </div>
  </div>
</template>

<script>
import Pawn from './Pawn.vue';

export default {
  components: { Pawn },
  props: ['players'],
  data() {
    return {
      cells: this.initializeBoard(),
      selectedCellIndex: null,
      currentPlayerIndex: 0,
    };
  },
  methods: {
    initializeBoard() {
      return Array(55).fill({ pawn: null, bonus: false })
        .map((cell, index) => {
          return { ...cell, bonus: (index + 1) % 10 === 0 };
        });
    },
    selectCell(index) {
      if (this.selectedCellIndex !== null) {
        this.movePawn(this.selectedCellIndex, index);
        this.selectedCellIndex = null;
      } else if (this.cells[index].pawn) {
        this.selectedCellIndex = index;
      }
    },
    movePawn(fromIndex, toIndex) {
      this.cells[toIndex].pawn = this.cells[fromIndex].pawn;
      this.cells[fromIndex].pawn = null;
      if (this.cells[toIndex].bonus) {
        this.$emit('bonus', this.currentPlayer);
      }
      this.nextPlayer();
    },
    nextPlayer() {
      this.currentPlayerIndex = (this.currentPlayerIndex + 1) % this.players.length;
    },
  },
  computed: {
    currentPlayer() {
      return this.players[this.currentPlayerIndex];
    },
  },
}
</script>

<style>
.board {
  display: grid;
  grid-template-columns: repeat(11, 100px);
  gap: 5px;
  margin: auto;
  padding: 20px;
}

.cell {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.cell.bonus {
  background-color: #f0f0f0;
}

.cell-label {
  font-size: 24px;
}
</style>
