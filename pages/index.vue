<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="auto" md="8">
        <h1 class="my-4">Hello visitor! 👋</h1>
        <p>
          I made this portfolio so you can understand who I am as a professional
          software developer. I want this website to be able to express the work
          and enthusiasm that I feel for programming. Since my beginnings, I
          realized that it is a continuous learning process that never ends so
          having a reference of my work was an obvious choice. This website has
          made developing a much better experience. Now I am able to find small
          pieces of code that have proven to be useful, also by displaying my
          progress to the outside world.
        </p>
        <p>This website is divided in five main sections.</p>
        <ul>
          <li>
            <v-icon>mdi-apps</v-icon>
            <b> App demos: </b>this section is about web application examples
            that display full stack integration technologies.
          </li>
          <li class="mt-3">
            <v-icon>mdi-web</v-icon>
            <b> Websites: </b> This section is about website designs that I have
            come to life.
          </li>
          <li class="mt-3">
            <v-icon>mdi-script-text</v-icon>
            <b>Articles: </b> this section is about walk-through setups for tech
            projects.
          </li>
          <li class="mt-3">
            <v-icon>mdi-script-text</v-icon>
            <b>Snippets: </b> this section is about different reusable pieces
            that have helped me through programming. I often use them as a
            reference when I am coding or when I forget commands.
          </li>
          <li class="mt-3">
            <v-icon>mdi-palette</v-icon>
            <b>Art: </b> this section is about art that I have created following
            art-by-uk tutorials
          </li>
        </ul>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style></style>
