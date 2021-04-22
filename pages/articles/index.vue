<template>
  <div>
    <div>
      <h1>Articles</h1>
      <ul>
        <li v-for="article of articles" :key="article.title">
          <pre>
            {{ article }}
          </pre>
          <NuxtLink :to="{ path: article.path }">
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
    const articles = await ctx
      .$content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .fetch()

    return {
      articles,
    }
  },
})
</script>
