<template>
  <v-app>
    <v-main class="d-flex align-center justify-center min-vh-100">
      <!-- Botão para ativar/desativar o modo de depuração -->
      <v-btn fixed bottom left @click="showDebugSidebar = !showDebugSidebar">
        Toggle Debug Sidebar
      </v-btn>

      <!-- Tela 1: Seleção do Modo -->
      <div v-if="mode === null" class="text-center">
        <v-btn @click="mode = 'cards'">Apenas Cartas</v-btn>
        <v-btn @click="mode = 'fullGame'">Jogo Completo</v-btn>
      </div>

      <!-- Tela 2: Sorteio de Carta -->
      <div v-else-if="mode === 'cards' && !drawnCard" class="text-center">
        <v-btn @click="drawCard">Sortear Carta</v-btn>
        <v-btn @click="mode = null">Voltar</v-btn>
      </div>

      <!-- Tela 3: Exibição da Carta Sorteada -->
      <div v-else-if="mode === 'cards' && drawnCard">
        <v-btn @click="mode = null">Voltar</v-btn>
        <Card
          style="margin: 0.1em"
          :card="drawnCard"
          @next="goToNextCard"
          @previous="goToPreviousCard"
        />
        <v-bottom-navigation style="display: flex; bottom: 0; margin: -1em">
          <v-btn @click.stop="showHistory = true">
            <v-icon>mdi-history</v-icon>
            <span>Recent</span>
          </v-btn>
        </v-bottom-navigation>
      </div>

      <!-- Tela 4: Jogo Completo (a ser implementado) -->
      <div v-else-if="mode === 'fullGame'">
        <Board />
      </div>

      <!-- Diálogo de Histórico -->
      <HistoryDialog :history="drawnCards" v-model="showHistory" />

      <!-- Snackbar para Mensagens -->
      <v-snackbar v-model="snackbar.showing" :timeout="3000" top>
        {{ snackbar.message }}
        <v-btn color="pink" text @click="snackbar.showing = false">
          Fechar
        </v-btn>
      </v-snackbar>
    </v-main>
    <DebugModeVue
      :cards="cards"
      v-model="showDebugSidebar"
      absolute
      temporary
    />
  </v-app>
</template>

<script>
import axios from 'axios'
import Card from '../components/Card.vue'
import HistoryDialog from '../components/HistoryDialog.vue'
import DebugModeVue from '../components/DebugMode.vue'
import Board from '../components/Board.vue'

export default {
  components: {
    Card,
    HistoryDialog,
    DebugModeVue,
    Board
  },
  data() {
    return {
      cards: [],
      drawnCards: [],
      drawnCard: null,
      mode: null,
      showHistory: false,
      currentCardIndex: 0,
      snackbar: { showing: false, message: '' },
      debugMode: process.env.VUE_APP_DEBUG === 'true',
      showDebugSidebar: false,
    }
  },
  mounted() {
    axios
      .get('/cards.json')
      .then((response) => {
        this.cards = response.data
      })
      .catch((error) => {
        console.error('Erro ao buscar cartas:', error)
        this.showSnackbar('Erro ao carregar as cartas.')
      })
  },
  methods: {
    drawCard() {
      if (this.cards.length === this.drawnCards.length) {
        this.showSnackbar('Todas as cartas já foram sorteadas!')
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
    showSnackbar(message) {
      this.snackbar.message = message
      this.snackbar.showing = true
    },
  },
}
</script>
