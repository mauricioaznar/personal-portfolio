<template>
  <v-container fluid class="flex-container">
    <v-row>
      <v-col>
        <h1>Art</h1>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col sm="12" md="auto">
        <img
          v-for="image of links"
          :key="image.name"
          :class="imageClass"
          :src="image.download_url"
          alt="triangle with all three sides equal"
        />
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
  computed: {
    imageClass() {
      return this.$vuetify.breakpoint.smAndDown ? 'img__big' : 'img__small'
    },
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

<style scoped>
.flex-container {
  flex-wrap: wrap;
}

.img__small {
  max-height: 15rem;
  margin: 1rem 2rem;
  min-height: 15rem;
}
.img__big {
  max-width: 80vw;
  max-height: 70vh;
  margin: 0.5rem 0;
}
</style>
