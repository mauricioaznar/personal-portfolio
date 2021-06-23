<template>
  <v-card elevation="6" class="fill-height" color="grey lighten-2">
    <div class="d-flex flex-no-wrap justify-space-between fill-height">
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title> Personal portfolio</v-card-title>

          <v-card-subtitle
            ><b>Description: </b>A website about my coding experience
          </v-card-subtitle>

          <v-card-text>
            <ul>
              <li>Markdown</li>
              <li>Page transitions</li>
              <li>Minimalistic design</li>
            </ul>
          </v-card-text>
        </div>
      </div>

      <div class="d-flex flex-wrap flex-column justify-center">
        <v-avatar class="ma-3 elevation-4" size="200" tile>
          <v-img
            src="/websites/img/personal-portfolio.png"
            position="center left"
          ></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'PersonalPortfolioCard',
}
</script>

<style scoped lang="scss"></style>
