<template>
  <v-app>
    <v-main class="d-flex align-center justify-center min-vh-100">
      <!-- Tela 1: Seleção do Modo -->
      <div v-if="mode === null" class="text-center">
        <v-btn @click="mode = 'cards'">Apenas Cartas</v-btn>
        <v-btn @click="mode = 'fullGame'">Jogo Completo</v-btn>
      </div>

      <!-- Tela 2: Sorteio de Carta -->
      <div v-else-if="mode === 'cards' && !drawnCard" class="text-center">
        <v-btn @click="drawCard">Sortear Carta</v-btn>
      </div>

      <!-- Tela 3: Exibição da Carta Sorteada -->
      <div v-else-if="mode === 'cards' && drawnCard">
        <Card
          style="margin: 0.1em"
          :card="drawnCard"
          @next="goToNextCard"
          @previous="goToPreviousCard"
        />
        <v-bottom-navigation style="display: flex, bottom: 0, margin -1em">
          <v-btn @click.stop="showHistory = true"
            ><v-icon>mdi-history</v-icon>

            <span>Recent</span>
          </v-btn>
        </v-bottom-navigation>
      </div>

      <!-- Tela 4: Jogo Completo (a ser implementado) -->
      <div v-else-if="mode === 'fullGame'">
        <!-- Lógica para o modo 'Jogo Completo' -->
      </div>

      <!-- Diálogo de Histórico -->
      <HistoryDialog :history="drawnCards" v-model="showHistory" />
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import Card from '../components/Card.vue'
import HistoryDialogVue from '../components/HistoryDialog.vue'

export default {
  components: {
    Card,
    HistoryDialogVue,
  },
  data() {
    return {
      cards: [],
      drawnCards: [],
      drawnCard: null,
      mode: null,
      showHistory: false,
      currentCardIndex: 0,
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
    goToNextCard() {
      this.currentCardIndex++
      if (this.currentCardIndex >= this.drawnCards.length) {
        this.drawCard()
      } else {
        this.drawnCard = this.drawnCards[this.currentCardIndex]
      }
    },
    goToPreviousCard() {
      if (this.currentCardIndex > 0) {
        this.currentCardIndex--
        this.drawnCard = this.drawnCards[this.currentCardIndex]
      }
    },
  },
}
</script>
