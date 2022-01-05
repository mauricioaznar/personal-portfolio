<template>
  <v-card dark class="pa-2">
    <div
      class="d-flex flex-column flex-md-row flex-no-wrap justify-space-between align-stretch"
    >
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title class="text-h5">
            <v-icon color="yellow darken-3"> mdi-star</v-icon>
            Notary
          </v-card-title>

          <v-card-subtitle
            ><b>Description: </b>Considerably the most robust demo I've ever
            made. It consists on handling information of a law firm
          </v-card-subtitle>
        </div>

        <v-divider />

        <v-card-text>
          <p class="text-h5">Most prominent features</p>
          <ul>
            <li>
              Calendar layout that updates with live data (appointments section)
            </li>
            <li>
              Stepper layout to handle many input fields (documents form
              section)
            </li>
            <li>
              User roles and groups to authorize users to certain actions (users
              and groups)
            </li>
            <li>
              Business logic tested on the server. Makes app considerably easier
              to grow/change (users and groups)
            </li>
            <li>
              Client and server validation. Improves user experience by giving
              correct and precise feedback
            </li>
            <li>Modern ui design by using google material principles.</li>
            <li>...and more</li>
          </ul>
        </v-card-text>

        <v-divider />

        <v-card-text>
          <p class="mt-2 text-h6">Technical details</p>
          <ul>
            <li>UI made with vue and vuetify. Hosted on netlfiy</li>
            <li>
              Server made with express, nestjs and mysql. Hosted on digitalocean
            </li>
          </ul>
        </v-card-text>

        <v-divider />

        <v-card-actions>
          <v-btn
            color="accent"
            link
            href="https://notary.mauaznar.com/"
            target="_blank"
          >
            Go to app
          </v-btn>
          <v-btn
            color="#4078c0"
            dark
            link
            href="https://github.com/mauricioaznar/vue-notary/"
            target="_blank"
          >
            UI
            <v-icon right>mdi-github</v-icon>
          </v-btn>
          <v-btn
            color="#4078c0"
            dark
            link
            href="https://github.com/mauricioaznar/nestjs-notary/"
            target="_blank"
          >
            API
            <v-icon right>mdi-github</v-icon>
          </v-btn>
        </v-card-actions>
      </div>

      <div
        class="d-flex flex-wrap flex-row flex-md-column justify-space-around"
      >
        <v-avatar class="ma-3" :size="smallDown ? 200 : 300" tile>
          <v-img src="/demos/img/notary-3-small.png"></v-img>
        </v-avatar>
        <v-avatar class="ma-3" :size="smallDown ? 200 : 300" tile>
          <v-img src="/demos/img/notary-1-small.png"></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'Notary',
  computed: {
    smallDown() {
      return this.$vuetify.breakpoint.xs
    },
  },
}
</script>

<style scoped></style>
