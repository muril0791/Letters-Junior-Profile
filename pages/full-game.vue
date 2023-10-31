<template>
  <v-container>
    <PlayerRegistration v-if="!gameStarted" @startGame="startGame"/>
    <Board v-else :players="players" />
  </v-container>
</template>

<script>
import Board from '../components/Board.vue'
import PlayerRegistration from '../components/PlayerRegistration.vue'

export default {
  components: { Board, PlayerRegistration },
  data() {
    return {
      gameStarted: false,
      players: [],
    };
  },
  methods: {
    startGame(players) {
      this.players = players.map(player => ({ ...player, position: 0 }));
      this.gameStarted = true;
    },
  },
}
</script>

<style>
/* Você pode adicionar estilos globais aqui se necessário */
</style>
