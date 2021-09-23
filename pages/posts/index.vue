<template>
  <v-container fluid>
    <v-row justify="center">
      <v-col sm="12" md="6">
        <div>
          <h1>Articles</h1>
          <ul>
            <li v-for="article of articles" :key="article.title">
              <NuxtLink
                :to="{
                  path: $route.path + article.path,
                }"
              >
                <div>
                  <h2>{{ article.title }}</h2>
                </div>
              </NuxtLink>
            </li>
          </ul>
        </div>
      </v-col>
      <v-col sm="12" md="6">
        <div>
          <h1>Snippets</h1>
          <ul>
            <li v-for="snippet of snippets" :key="snippet.title">
              <NuxtLink
                :to="{
                  path: $route.path + snippet.path,
                }"
              >
                <div>
                  <h2>{{ snippet.title }}</h2>
                </div>
              </NuxtLink>
            </li>
          </ul>
        </div>
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

    const articles = await retrievePosts('articles')
    const snippets = await retrievePosts('snippets')

    return {
      articles,
      snippets,
    }
  },
})
</script>
