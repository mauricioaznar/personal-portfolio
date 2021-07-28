<template>
  <v-container fluid>
    <v-row justify="center">
      <v-col sm="auto" md="8">
        <div>
          <h1>Art</h1>
          <div v-for="image of links" :key="image.name">
            <img
              :src="image.download_url"
              alt="triangle with all three sides equal"
              width="100%"
            />
          </div>
        </div>
        <NuxtChild />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      links: [],
    }
  },
  created() {
    this.customFetch()
  },
  methods: {
    async customFetch() {
      const result = await this.$axios.$get(
        'https://api.github.com/repos/mauricioaznar/inkscape/contents'
      )
      this.links = result.filter((item) => {
        return item.name.includes('svg')
      })
    },
  },
})
</script>
