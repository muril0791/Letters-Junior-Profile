<template>
  <div class="game-container">
    <div class="board">
      <div
        v-for="(cell, index) in playersInCells"
        :key="index"
        class="cell"
        :class="{
          bonus: cell.bonus,
          blue: cell.color === 'blue',
          orange: cell.color === 'orange',
        }"
        @click="selectCell(index)"
      >
        <pawn
          v-for="(player, index) in players"
          :key="index"
          :player="cell.player"
        />
        <div class="cell-label" v-if="cell.bonus">?</div>
      </div>
    </div>
    <div class="player-list">
      <div v-for="(player, index) in players" :key="index" class="player-item">
        {{ player.name }} ({{ player.color }}) - {{ player.position }}
      </div>
    </div>
    <v-btn @click="nextPlayer">Next Player</v-btn>
    <v-btn @click="startGame">Start Game</v-btn>
    <div v-if="currentCard">
      <!-- Assuming you have a card component to display the current card -->
      <card :data="currentCard" />
    </div>
  </div>
</template>

<script>
import Pawn from './Pawn.vue';
import Card from '../pages/cards.vue';

export default {
  components: { Pawn, Card },
  props: ['players'],
  data() {
    return {
      cells: this.initializeBoard(),
      selectedCellIndex: null,
      currentPlayerIndex: 0,
      gameStarted: false,
      currentCard: null,
      hintsUsed: 0,  // Number of hints used in the current round
    };
  },
  computed: {
    playersInCells() {
      return this.cells.map(cell => {
        if (cell.pawn) {
          const player = this.players.find(player => player.name === cell.pawn.playerName);
          return { ...cell, player };
        }
        return cell;
      });
    },
  },
  methods: {
    initializeBoard() {
      return Array(55)
        .fill({ pawn: null, bonus: false, color: 'blue' })
        .map((cell, index) => {
          return {
            ...cell,
            bonus: (index + 1) % 10 === 0,
            color: index % 2 === 0 ? 'blue' : 'orange',
          }
        })
    },
    selectCell(index) {
      if (this.gameStarted) {
        this.movePawn(this.selectedCellIndex, index)
        this.selectedCellIndex = null
      }
    },
    movePawn(fromIndex, toIndex) {
      const pawn = this.cells[fromIndex].pawn
      this.$set(this.cells, fromIndex, { ...this.cells[fromIndex], pawn: null })
      this.$set(this.cells, toIndex, { ...this.cells[toIndex], pawn })
    },
    nextPlayer() {
      this.currentPlayerIndex =
        (this.currentPlayerIndex + 1) % this.players.length
      this.drawNewCard();  // Draw a new card for the next player
    },
    startGame() {
      this.gameStarted = true;
      this.currentPlayerIndex = 0;
      this.players.forEach((player, index) => {
        this.$set(this.cells[index * 2], 'pawn', {
          color: player.color,
          playerName: player.name,
        });
      });
      this.drawNewCard();
    },
    drawNewCard() {
      // Assume you have a method to get a new card
      this.currentCard = {
        title: 'New Card Title',
        category: 'New Card Category',
        hints: ['Hint 1', 'Hint 2', 'Hint 3'],
      };
    },
    cardFinished(hintsUsed) {
      const steps = hintsUsed <= 19 ? hintsUsed : 0;
      this.movePlayer(steps);
      this.nextPlayer();
    },
    movePlayer(steps) {
      const currentPlayer = this.players[this.currentPlayerIndex];
      const currentPosition = this.cells.findIndex(cell => cell.pawn && cell.pawn.playerName === currentPlayer.name);
      let newPosition = currentPosition + steps;
      newPosition = newPosition >= 55 ? 54 : newPosition;  // Ensure the position is within the board
      this.movePawn(currentPosition, newPosition);
    },
  },
}
</script>

<style>
.game-container {
  display: flex;
}

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

.cell.blue {
  background-color: #00f;
}

.cell.orange {
  background-color: #ffa500;
}

.cell-label {
  font-size: 24px;
}

.player-list {
  margin-left: 20px;
}

.player-item {
  margin-bottom: 10px;
}
</style>
