<template>
  <v-container fluid>
    <v-tabs v-model='tab' class='mb-4'>
      <v-tab>Articles</v-tab>
      <v-tab>Snippets</v-tab>
    </v-tabs>
    <v-tabs-items v-model='tab'>
      <v-tab-item>
        <v-row justify='start'>
          <v-col sm='12'>
            <h1>Articles</h1>
          </v-col>
          <v-col v-for='snippet of articles' :key='snippet.title' sm='12' md='6'>
            <NuxtLink
              :to='{

                  path: $route.path + snippet.path,
                }'
              style='text-decoration: none'
              class='d-flex'
            >
              <v-icon>mdi-circle-small</v-icon>
              <h2>{{ snippet.title }}</h2>
            </NuxtLink>
          </v-col>
        </v-row>
      </v-tab-item>
      <v-tab-item>
        <v-row justify='start'>
          <v-col sm='12'>
            <h1>Snippets</h1>
          </v-col>
          <v-col v-for='snippet of snippets' :key='snippet.title' sm='12' md='6'>
            <NuxtLink
              :to='{

                  path: $route.path + snippet.path,
                }'
              style='text-decoration: none'
              class='d-flex'
            >
              <v-icon>mdi-circle-small</v-icon>
              <h2>{{ snippet.title }}</h2>
            </NuxtLink>
          </v-col>
        </v-row>
      </v-tab-item>
    </v-tabs-items>

  </v-container>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      tab: 0
    }
  },
  watch: {
    tab() {
      window.localStorage.setItem('post_tab', this.tab)
    }
  },
  mounted() {
    const postTab = window.localStorage.getItem('post_tab')
    this.tab = postTab ? Number(postTab) : 0
  },
  async asyncData(ctx) {
    const retrievePosts = async (category) => {
      let posts = []
      try {
        posts = await ctx.$content(category).sortBy('createdAt', 'asc').fetch()
      } catch (e) {
        ctx.error({
          statusCode: 404,
          message: 'Message not found'
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


    const articles = await retrievePosts('articles')
    const snippets = await retrievePosts('snippets')

    return {
      articles: articles.sort(compare),
      snippets: snippets.sort(compare),
    }
  }
})
</script>

<style>

</style>
