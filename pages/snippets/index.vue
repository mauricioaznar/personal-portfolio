<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="auto" md="6">
        <div>
          <h1>Snippets</h1>
          <ul>
            <li v-for="snippet of snippets" :key="snippet.title">
              <NuxtLink :to="{ path: $route.path + '/' + snippet.slug }">
                <div>
                  <h2>{{ snippet.title }}</h2>
                </div>
              </NuxtLink>
            </li>
          </ul>
        </div>
        <NuxtChild />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Vue from 'vue'

export default Vue.extend({
  async asyncData(ctx) {
    let snippets = []
    try {
      snippets = await ctx
        .$content('snippets')
        .sortBy('createdAt', 'asc')
        .fetch()
    } catch (e) {
      ctx.error({
        statusCode: 404,
        message: 'Message not found',
      })
    }

    return {
      snippets,
    }
  },
})
</script>
