<template>
  <v-app>
    <div>
      <v-switch v-model="$vuetify.theme.dark" label="Dark mode"></v-switch>
    </div>
    <v-main class="d-flex align-center justify-center min-vh-100">
      <div class="text-center">
        <v-btn @click="goToCards">Apenas Cartas</v-btn>
        <v-btn @click="goToFullGame">Jogo Completo</v-btn>
      </div>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {}
  },
  methods: {
    goToCards() {
      this.$router.push('/cards')
    },
    goToFullGame() {
      this.$router.push('/full-game')
    },
  },
}
</script>
