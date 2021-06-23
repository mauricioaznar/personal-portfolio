<template>
  <v-container>
    <v-row>
      <v-col sm="12" md="6" lg="4">
        <NatoursCard />
      </v-col>

      <v-col sm="12" md="6" lg="4">
        <TrilloCard />
      </v-col>

      <v-col sm="12" md="6" lg="4">
        <NexterCard />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import NatoursCard from './cards/natours.vue'
import TrilloCard from './cards/trillo.vue'
import NexterCard from './cards/nexter.vue'

export default {
  components: { TrilloCard, NatoursCard, NexterCard },
}
</script>

<style scoped></style>
