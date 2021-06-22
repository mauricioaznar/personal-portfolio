<template>
  <v-container>
    <v-row>
      <v-col sm="12" md="6"> <NatoursCard /> </v-col>
    </v-row>
  </v-container>
</template>

<script>
import NatoursCard from './cards/Natours'

export default {
  components: { NatoursCard },
}
</script>

<style scoped></style>
