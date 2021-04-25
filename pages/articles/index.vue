<template>
  <div>
    <div>
      <h1>Articles</h1>
      <ul>
        <li v-for="article of articles" :key="article.title">
          <NuxtLink :to="{ path: $route.path + '/' + article.slug }">
            <div>
              <h2>{{ article.title }}</h2>
            </div>
          </NuxtLink>
        </li>
      </ul>
    </div>
    <NuxtChild />
  </div>
</template>

<script>
import Vue from 'vue'
export default Vue.extend({
  async asyncData(ctx) {
    let articles = []
    try {
      articles = await ctx
        .$content('articles')
        .sortBy('createdAt', 'asc')
        .fetch()
    } catch (e) {
      ctx.error({
        statusCode: 404,
        message: 'Message not found',
      })
    }

    return {
      articles,
    }
  },
})
</script>
