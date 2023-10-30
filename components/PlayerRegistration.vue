<template>
  <div class="player-registration">
    <v-text-field v-model="playerName" label="Nome do Jogador" />
    <v-btn @click="registerPlayer">Registrar Jogador</v-btn>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerName: ''
    };
  },
  methods: {
    registerPlayer() {
      if (this.playerName) {
        this.$emit('register', { name: this.playerName });
        this.playerName = '';
      }
    }
  }
}
</script>
