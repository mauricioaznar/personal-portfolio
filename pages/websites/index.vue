<template>
  <v-container fluid>
    <v-row>
      <v-col sm="12" md="6" lg="6" xl="4">
        <NatoursCard />
      </v-col>

      <v-col sm="12" md="6" lg="6" xl="4">
        <TrilloCard />
      </v-col>

      <v-col sm="12" md="6" lg="6" xl="4">
        <NexterCard />
      </v-col>

      <v-col sm="12" md="6" lg="6" xl="4">
        <SieteCenotesCard />
      </v-col>

      <v-col sm="12" md="6" lg="6" xl="4">
        <PersonalPortfolioCard />
      </v-col>

      <v-col sm="12" md="6" lg="6" xl="4">
        <GatsbyCard />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import NatoursCard from './cards/natours.vue'
import TrilloCard from './cards/trillo.vue'
import NexterCard from './cards/nexter.vue'
import SieteCenotesCard from './cards/7cenotes'
import PersonalPortfolioCard from './cards/personal-portfolio'
import GatsbyCard from './cards/gatsby'

export default {
  components: {
    GatsbyCard,
    PersonalPortfolioCard,
    SieteCenotesCard,
    TrilloCard,
    NatoursCard,
    NexterCard,
  },
}
</script>

<style scoped></style>
