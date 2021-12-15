<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="12" md="6">
        <v-row>
          <v-col sm="12">
            <h1>{{ title }}</h1>
          </v-col>
        </v-row>
        <v-row justify="center">
          <v-col v-for="snippet of posts" :key="snippet.title" sm="12" md="12">
            <NuxtLink
              :to="{
                path: $route.path + '/' + snippet.slug,
              }"
              style="text-decoration: none"
              class="d-flex"
            >
              <v-icon>mdi-circle-small</v-icon>
              <h2>{{ snippet.title }}</h2>
              <span>{{ log(snippet) }}</span>
            </NuxtLink>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  async asyncData(ctx) {
    const retrievePosts = async (category) => {
      let posts = []
      try {
        posts = await ctx.$content(category).sortBy('createdAt', 'asc').fetch()
      } catch (e) {
        ctx.error({
          statusCode: 404,
          message: 'Message not found',
        })
      }
      return posts
    }

    const compare = (a, b) => {
      if (a.title.toLowerCase() < b.title.toLowerCase()) {
        return -1
      }
      if (a.title.toLowerCase() > b.title.toLowerCase()) {
        return 1
      }
      return 0
    }

    const category = ctx.params.category

    const posts = await retrievePosts(category)

    return {
      posts: posts.sort(compare),
      title: category.charAt(0).toUpperCase() + category.slice(1),
    }
  },
  methods: {
    log(item) {
      console.log(item)
      return ''
    },
  },
})
</script>

<style></style>
