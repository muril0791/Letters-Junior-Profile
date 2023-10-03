<template>
  <v-navigation-drawer  v-model="value" absolute temporary>
    <v-list class="debug">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>Total de Cartas</v-list-item-title>
          <v-list-item-subtitle>{{ cards.length }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
     <v-list-item>
        <v-list-item-content>
          <v-list-item-title>Cartas Repetidas</v-list-item-title>
          <v-list-item-subtitle>
            <div class="chip-container">
              <!-- Lista de títulos das cartas repetidas -->
              <v-chip v-for="card in repeatedCards" :key="card.title" small>
                {{ card.title }}
              </v-chip>
            </div>
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
  props: ['cards', 'value'],
  computed: {
    repeatedCards() {
      const titleCount = {}
      this.cards.forEach(card => {
        if (titleCount[card.title]) {
          titleCount[card.title]++
        } else {
          titleCount[card.title] = 1
        }
      })
      return this.cards.filter(card => titleCount[card.title] > 1)
    }
  }
}
</script>
<style scoped>
.debug{
    height: 100%;
}
.chip-container > .v-chip {
  display: block;
  margin-bottom: 4px; 
}
</style>