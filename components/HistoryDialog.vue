<template>
  <v-dialog v-model="value" max-width="290">
    <v-card>
      <v-card-title class="headline">Histórico de Cartas</v-card-title>
      <v-card-text>
        <v-list>
          <v-list-item v-for="(card, index) in history" :key="index">
            <v-list-item-content>
              <v-list-item-title>{{ card.title }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="value = false">Fechar</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: ['history', 'value'],
  data() {
    return {
     value: false // Remova esta linha
    }
  }
}
</script>
