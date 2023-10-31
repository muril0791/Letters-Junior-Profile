<template>
  <div class="player-registration">
    <v-text-field v-model="playerName" label="Nome do Jogador" />
    <v-color-picker v-model="color" dot-size="30" />
    <v-btn @click="register">Registrar</v-btn>
    <v-btn @click="startGame">Iniciar Jogo</v-btn>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerName: '',
      color: '',
      players: [],
    };
  },
  methods: {
    register() {
      this.players.push({ playerName: this.playerName, color: this.color, position: 0 });
      this.playerName = '';
      this.color = '';
    },
    startGame() {
      this.$emit('startGame', this.players);
    },
  },
}
</script>

<style>
.player-registration {
  display: flex;
  flex-direction: column;
}
</style>
