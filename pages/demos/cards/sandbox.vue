<template>
  <v-card color="red darken-4" dark class="pa-2">
    <div
      class="d-flex flex-no-wrap flex-column flex-md-row justify-space-between align-stretch"
    >
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title class="text-h5"> Sandbox </v-card-title>

          <v-card-subtitle
            ><b>Description: </b>App made to test graphql and react features.
            <br />
          </v-card-subtitle>
        </div>

        <v-divider />

        <v-card-text>
          <p class="text-h5">Most prominent features</p>
          <ul>
            <li>Image upload</li>
            <li>Authorization</li>
            <li>Graphql authorized subscriptions</li>
            <li>...and more</li>
          </ul>
        </v-card-text>

        <v-divider />

        <v-card-text>
          <p class="mt-2 text-h6">Technical details</p>
          <ul>
            <li>UI made with react and material ui. Hosted on netlfiy</li>
            <li>
              Server api made with nestjs and mongodb. Hosted on
              digitalocean
            </li>
            <li>Apollo across front and back end.</li>
          </ul>
        </v-card-text>

        <v-divider />

        <v-card-actions class="d-flex flex-wrap flex-row">
          <v-row dense>
            <v-col cols="auto">
              <v-btn
                color="accent"
                link
                href="https://sandbox.mauaznar.com/"
                target="_blank"
              >
                Go to app
              </v-btn>
            </v-col>
            <v-col cols="auto">
              <v-btn
                color="#4078c0"
                dark
                link
                href="https://github.com/mauricioaznar/react-s-todo/"
                target="_blank"
              >
                Client
                <v-icon right>mdi-github</v-icon>
              </v-btn>
            </v-col>
            <v-col cols="auto">
              <v-btn
                color="#4078c0"
                dark
                link
                href="https://github.com/mauricioaznar/nestjs-s-todo/"
                target="_blank"
              >
                Server
                <v-icon right>mdi-github</v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </div>

      <div
        class="d-flex flex-wrap flex-row flex-md-column justify-space-around"
      >
        <v-avatar class="ma-3" :size="smallDown ? 200 : 300" tile>
          <v-img
            src="/demos/img/performance-1-small.png"
            gradient="to top right, rgba(100,115,201,.2), rgba(25,32,72,.4)"
          ></v-img>
        </v-avatar>
        <v-avatar class="ma-3" :size="smallDown ? 200 : 300" tile>
          <v-img
            position="95% 50%"
            src="/demos/img/performance-2-small.png"
            gradient="to top right, rgba(100,115,201,.2), rgba(25,32,72,.4)"
          ></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'Performance',
  computed: {
    smallDown() {
      return this.$vuetify.breakpoint.xs
    },
  },
}
</script>

<style scoped></style>
