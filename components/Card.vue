<template>
  <v-row align="center" justify="center">
    <v-card class="mx-auto ma-2" color="indigo">
      <v-card-item>
        <div>
          <div class="text-h6 mb-1" align="center">
            Diga os outros que sou um:
            {{ card.category }}
          </div>
          <div class="text-overline mb-1" align="center">
            <span v-if="showTitle">{{ card.title }}</span>
            <span v-else>******</span>
            <v-btn icon @click="showTitle = !showTitle">
              <v-icon>{{ showTitle ? 'mdi-eye-off' : 'mdi-eye' }}</v-icon>
            </v-btn>
          </div>
          <div class="text-caption" align="center">
            <ol>
              <li v-for="(hint, index) in card.hints" :key="index">
                <v-checkbox v-model="hintsGiven[index]" :label="hint" hide-details></v-checkbox>
              </li>
            </ol>
          </div>
        </div>
      </v-card-item>
    </v-card>
  </v-row>
</template>

<script>
export default {
  name: 'AppCard',
  props: ['card'],
  data() {
    return {
      hintsGiven: [],
      showTitle: false
    }
  },
  watch: {
    card: {
      handler() {
        this.hintsGiven = new Array(this.card.hints.length).fill(false);
      },
      immediate: true
    }
  }
}
</script>

<style scoped>
.card {
  text-align: center;
}
</style>
