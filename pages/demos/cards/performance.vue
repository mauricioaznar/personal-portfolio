<template>
  <v-card color="blue darken-4" dark class="pa-2">
    <div
      class="d-flex flex-no-wrap flex-column flex-md-row justify-space-between align-stretch"
    >
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title class="text-h5"> Performance monitor </v-card-title>

          <v-card-subtitle
            ><b>Description: </b>App made to montitor cpu, memory and disk usage
            by sending live data.
            <br />
            <i
              >Credits to: Robert Bunch (<a
                href="https://www.udemy.com/course/socketio-with-websockets-the-details/"
                >Socket IO Course</a
              >)
            </i>
          </v-card-subtitle>
        </div>

        <v-divider />

        <v-card-text>
          <p class="text-h5">Most prominent features</p>
          <ul>
            <li>Updates ui each time new data is being sent from a client</li>
            <li>A client sends memory, cpu and disk usage each 100ms</li>
            <li>
              Data is handled by animating certain states. Example: when memory
              displays a high usage, it changes to a red color
            </li>
            <li>...and more</li>
          </ul>
        </v-card-text>

        <v-divider />

        <v-card-text>
          <p class="mt-2 text-h6">Technical details</p>
          <ul>
            <li>UI made with react and chakra ui. Hosted on netlfiy</li>
            <li>
              Client made with express. Hosted on the desired machine (mac,
              windows or linux)
            </li>
            <li>
              Server api made with express, nestjs and mongodb. Hosted on
              digitalocean
            </li>
            <li>Server database hosted on mongoatlas</li>
          </ul>
        </v-card-text>

        <v-divider />

        <v-card-actions class="d-flex flex-wrap flex-row">
          <v-row dense>
            <v-col cols="auto">
              <v-btn
                color="accent"
                link
                href="https://performance.mauaznar.com/"
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
                href="https://github.com/mauricioaznar/performance-ui/"
                target="_blank"
              >
                UI
                <v-icon right>mdi-github</v-icon>
              </v-btn>
            </v-col>
            <v-col cols="auto">
              <v-btn
                color="#4078c0"
                dark
                link
                href="https://github.com/mauricioaznar/performance-client/"
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
                href="https://github.com/mauricioaznar/performance-server/"
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
