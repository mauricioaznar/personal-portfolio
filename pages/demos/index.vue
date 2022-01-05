<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="12" md="12" lg="9" xl="12">
        <v-container>
          <v-row>
            <v-col sm="12" xl="6">
              <Notary />
            </v-col>

            <v-col sm="12" xl="6">
              <Performance />
            </v-col>

            <v-col sm="12" xl="6">
              <Sandbox />
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Notary from '@/pages/demos/cards/notary'
import Performance from '@/pages/demos/cards/performance'
import Sandbox from '@/pages/demos/cards/sandbox'
export default {
  components: { Performance, Notary, Sandbox },
}
</script>

<style scoped></style>
