<template>
  <v-container>
    <v-row>
      <v-col>
        <v-card dark>
          <div class="d-flex flex-no-wrap justify-space-between align-stretch">
            <div class="flex-grow-1 d-flex flex-column justify-space-between">
              <div>
                <v-card-title class="text-h5">
                  <v-icon color="yellow darken-3"> mdi-star </v-icon>
                  Notary
                </v-card-title>

                <v-card-subtitle
                  ><b>Description: </b>Considerably the most robust demo I've
                  ever made. It consists on handling information of a law
                  firm</v-card-subtitle
                >
              </div>

              <v-divider />

              <v-card-text>
                <p class="text-h5">Most prominent features</p>
                <ul>
                  <li>
                    Calendar layout that updates with live data (appointments
                    section)
                  </li>
                  <li>
                    Stepper layout to handle many input fields (documents form
                    section)
                  </li>
                  <li>
                    User roles and groups to authorize users to certain actions
                    (users and groups)
                  </li>
                  <li>
                    Business logic tested on the server. Makes app considerably
                    easier to grow/change (users and groups)
                  </li>
                  <li>
                    Client and server validation. Improves user experience by
                    giving correct and precise feedback
                  </li>
                  <li>Modern ui design by using google material principles.</li>
                  <li>...and more</li>
                </ul>
                <p class="mt-2 text-h6">Technical details</p>
                <ul>
                  <li>UI made with vue and vuetify. Hosted on netlfiy</li>
                  <li>
                    Server made with express, nestjs and mysql. Hosted on
                    digitalocean
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

            <div class="d-flex flex-wrap flex-column">
              <v-avatar class="ma-3 ml-0" size="300" tile>
                <v-img src="/demos/img/img.png"></v-img>
              </v-avatar>
              <v-avatar class="ma-3 ml-0" size="300" tile>
                <v-img src="/demos/img/img.png"></v-img>
              </v-avatar>
            </div>
          </div>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="mt-6">
      <v-col>
        <v-card color="blue darken-4" dark>
          <div class="d-flex flex-no-wrap justify-space-between align-stretch">
            <div class="flex-grow-1 d-flex flex-column justify-space-between">
              <div>
                <v-card-title class="text-h5">
                  Performance monitor
                </v-card-title>

                <v-card-subtitle
                  ><b>Description: </b>App made to montitor cpu, memory and disk
                  usage by sending live data</v-card-subtitle
                >
              </div>

              <v-divider />

              <v-card-text>
                <p class="text-h5">Most prominent features</p>
                <ul>
                  <li>
                    Updates ui each time new data is being sent from a client
                  </li>
                  <li>A client sends memory, cpu and disk usage each 100ms</li>
                  <li>
                    Data is handled by animating certain states. Example: when
                    memory displays a high usage, it changes to a red color
                  </li>
                  <li>...and more</li>
                </ul>
                <p class="mt-2 text-h6">Technical details</p>
                <ul>
                  <li>UI made with react and chakra ui. Hosted on netlfiy</li>
                  <li>
                    Client made with express. Hosted on the desired machine
                    (mac, windows or linux)
                  </li>
                  <li>
                    Server api made with express, nestjs and mongodb. Hosted on
                    digitalocean
                  </li>
                  <li>Server database hosted on mongoatlas</li>
                </ul>
              </v-card-text>

              <v-divider />

              <v-card-actions>
                <v-btn
                  color="accent"
                  link
                  href="https://performance.mauaznar.com/"
                  target="_blank"
                >
                  Go to app
                </v-btn>
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
              </v-card-actions>
            </div>

            <div class="d-flex flex-wrap flex-column">
              <v-avatar class="ma-3 ml-0" size="300" tile>
                <v-img
                  src="/demos/img/performance.png"
                  gradient="to top right, rgba(100,115,201,.2), rgba(25,32,72,.4)"
                ></v-img>
              </v-avatar>
              <v-avatar class="ma-3 ml-0" size="300" tile>
                <v-img
                  position="95% 50%"
                  src="/demos/img/performance-zoom.png"
                  gradient="to top right, rgba(100,115,201,.2), rgba(25,32,72,.4)"
                ></v-img>
              </v-avatar>
            </div>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {}
</script>

<style scoped></style>
