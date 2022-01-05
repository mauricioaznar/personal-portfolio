<template>
  <v-card elevation="6" class="fill-height" color="#C07FE4" dark>
    <div class="d-flex flex-no-wrap justify-space-between fill-height">
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title>
            <v-img
              class="mr-2"
              max-height="30"
              max-width="30"
              src="/websites/img/gatsby-logo.png"
              position="center left"
            ></v-img>
            Gatsby
          </v-card-title>

          <v-card-text>
            <ul>
              <li>Markdown</li>
              <li>Optimized for search engines</li>
              <li>Page transitions</li>
              <li>Server side rendering</li>
              <li>Made with tailwindcss</li>
            </ul>
          </v-card-text>
        </div>
        <div>
          <v-divider />
          <v-card-actions>
            <v-btn
              dark
              color="#663299"
              link
              href="https://gatsby.mauaznar.com/"
              target="_blank"
            >
              Go to website
            </v-btn>
          </v-card-actions>
        </div>
      </div>

      <div class="d-flex flex-wrap flex-column justify-center">
        <v-avatar class="ma-3 elevation-4" size="200" tile>
          <v-img
            src="/websites/img/gatsby-small.png"
            position="center left"
          ></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'GatsbyCard',
}
</script>

<style scoped lang="scss"></style>
