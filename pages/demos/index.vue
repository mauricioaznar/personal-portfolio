<template>
  <v-container>
    <v-row>
      <v-col>
        <v-card>
          <div class="d-flex flex-no-wrap justify-space-between align-stretch">
            <div class="flex-grow-1 d-flex flex-column justify-space-between">
              <div>
                <v-card-title class="text-h5">
                  <v-icon color="yellow darken-3"> mdi-star </v-icon>
                  Notary
                </v-card-title>

                <v-card-subtitle>Document administration app</v-card-subtitle>
              </div>

              <v-divider />

              <v-card-text>
                <div>Word of the Day</div>
                <p class="text-h4 text--primary">be•nev•o•lent</p>
                <p>adjective</p>
                <div class="text--primary">
                  well meaning and kindly.<br />
                  "a benevolent smile"
                </div>
                <div class="text--primary">
                  well meaning and kindly.<br />
                  "a benevolent smile"
                </div>
                <div class="text--primary">
                  well meaning and kindly.<br />
                  "a benevolent smile"
                </div>
                <div class="text--primary">
                  well meaning and kindly.<br />
                  "a benevolent smile"
                </div>
                <div class="text--primary">
                  well meaning and kindly.<br />
                  "a benevolent smile"
                </div>
              </v-card-text>

              <v-divider />

              <v-card-actions>
                <v-btn
                  class="ml-2 mt-3"
                  fab
                  icon
                  height="40px"
                  right
                  width="40px"
                >
                  <v-icon>mdi-play</v-icon>
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
  </v-container>
</template>

<script>
export default {}
</script>

<style scoped></style>
