<template>
  <div class="container">
    <div v-if="!drawnCard" class="text-center button-container">
      <v-btn @click="drawCard">Sortear Carta</v-btn>
      <v-btn @click="goBack">Voltar</v-btn>
    </div>
    <div v-else class="card-container">
      <v-btn @click="goBack">Voltar</v-btn>
      <Card
        :card="drawnCard"
        @next="goToNextCard"
        @previous="goToPreviousCard"
      />
    </div>
    <HistoryDialog :history="drawnCards" v-model="showHistory" />
    <v-snackbar v-model="snackbar.showing" :timeout="3000" top>
      {{ snackbar.message }}
      <v-btn color="pink" text @click="snackbar.showing = false">
        Fechar
      </v-btn>
    </v-snackbar>
  </div>
</template>

<script>
import axios from 'axios';
import Card from '../components/cardComponent.vue';
import HistoryDialog from '~/components/HistoryDialog.vue';

export default {
  components: {
    Card,
    HistoryDialog
  },
  data() {
    return {
      cards: [],
      drawnCards: [],
      drawnCard: null,
      showHistory: false,
      currentCardIndex: 0,
      snackbar: { showing: false, message: '' }
    }
  },
  mounted() {
    axios.get('/cards.json').then(response => {
      this.cards = response.data;
    }).catch(error => {
      this.showSnackbar('Erro ao carregar as cartas.');
    });
  },
  methods: {
    goBack() {
      this.$router.push('/');
    },
    drawCard() {
      if (this.cards.length === this.drawnCards.length) {
        this.showSnackbar('Todas as cartas já foram sorteadas!');
        return;
      }
      let card;
      do {
        card = this.cards[Math.floor(Math.random() * this.cards.length)];
      } while (this.drawnCards.includes(card));
      this.drawnCards.unshift(card);
      this.drawnCard = card;
    },
    goToNextCard() {
      this.currentCardIndex++;
      if (this.currentCardIndex >= this.drawnCards.length) {
        this.drawCard();
      } else {
        this.drawnCard = this.drawnCards[this.currentCardIndex];
      }
    },
    goToPreviousCard() {
      if (this.currentCardIndex > 0) {
        this.currentCardIndex--;
        this.drawnCard = this.drawnCards[this.currentCardIndex];
      }
    },
    showSnackbar(message) {
      this.snackbar.message = message;
      this.snackbar.showing = true;
    },
  }
}
</script>

<style scoped>
.container {
  padding: 1em;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.button-container {
  display: flex;
  flex-direction: column;
  gap: 1em;
  margin-top: 2em;
}

.card-container {
  width: 100%;
  max-width: 500px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 2em;
}

.bottom-nav {
  width: 100%;
  position: fixed;
  bottom: 0;
  left: 0;
}

.text-center {
  text-align: center;
  width: 100%;
}

@media (max-width: 600px) {
  .button-container, .card-container {
    margin-top: 1em;
  }
}
</style>
