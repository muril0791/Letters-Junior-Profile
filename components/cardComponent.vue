<template>
  <v-row align="center" justify="center">
    <v-card class="mx-auto ma-2 card">
      <v-card-item> </v-card-item>
      <v-card-actions class="justify-space-between">
        <v-btn icon @click="$emit('previous')">
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>
        <v-btn icon @click="$emit('next')">
          <v-icon>mdi-arrow-right</v-icon>
        </v-btn>
      </v-card-actions>
      <div>
        <div class="text-h6 mb-1" align="center">
          Diga aos outros que sou um:
          <a style="font-size: 14px, text-color: white"> {{ card.category }}</a>
        </div>
        <div class="text-overline mb-1 ma-1" align="center">
          <span v-if="showTitle">{{ card.title }}</span>
          <span v-else>******</span>
          <v-btn icon @click="showTitle = !showTitle">
            <v-icon>{{ showTitle ? 'mdi-eye-off' : 'mdi-eye' }}</v-icon>
          </v-btn>
        </div>
        <div class="text-caption" align="center">
          <ol>
            <li v-for="(hint, index) in card.hints" :key="index">
              <v-checkbox
                v-model="hintsGiven[index]"
                :label="hint"
                hide-details
              ></v-checkbox>
            </li>
          </ol>
        </div>
      </div>
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
      showTitle: false,
    }
  },
  watch: {
    card: {
      handler() {
        this.hintsGiven = new Array(this.card.hints.length).fill(false)
      },
      immediate: true,
    },
  },
}
</script>

<style scoped>
.card {
  max-width: 90%;
  margin: 0 auto 1em auto;
  border: 1px solid rgba(0,0,0,0.1);
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: box-shadow 0.3s ease-in-out;
}

.card:hover {
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

@media only screen and (min-width: 600px) {
  .card {
    max-width: 400px;
  }
}
</style>
s