<template>
  <v-app>
    <v-navigation-drawer app>
      <v-list>
        <v-list-item-group v-if="drawnCards.length" v-model="selectedCard">
          <v-list-item v-for="(card, index) in drawnCards" :key="index">
            <v-list-item-content>
              <v-list-item-title>{{ card.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
        <v-list-item v-else>
          <v-list-item-content>Nenhum histórico disponível</v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-btn @click="mode = 'cards'">Apenas Cartas</v-btn>
      <v-btn @click="mode = 'fullGame'">Jogo Completo</v-btn>

      <div v-if="mode === 'cards'">
        <v-btn @click="drawCard">Sortear Carta</v-btn>
        <Card v-if="drawnCard" :card="drawnCard" />
      </div>
      <div v-else-if="mode === 'fullGame'">
        <!-- Lógica para o modo 'Jogo Completo' -->
      </div>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import Card from '../components/Card.vue'

export default {
  components: {
    Card,
  },
  data() {
    return {
      cards: [],
      drawnCards: [],
      drawnCard: null,
      mode: null,
      selectedCard: null,
    }
  },
  mounted() {
    axios.get('/cards.json').then((response) => {
      this.cards = response.data
    })
  },
  methods: {
    drawCard() {
      if (this.cards.length === this.drawnCards.length) {
        alert('Todas as cartas já foram sorteadas!')
        return
      }

      let card
      do {
        card = this.cards[Math.floor(Math.random() * this.cards.length)]
      } while (this.drawnCards.includes(card))

      this.drawnCards.unshift(card)
      this.drawnCard = card
    },
  },
}
</script>
