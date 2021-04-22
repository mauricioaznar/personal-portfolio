<template>
  <div>
    <h1>Hello Nuxters! 👋</h1>
    <p>
      This page is rendered on the <strong>{{ rendering }}</strong>
    </p>
    <p v-if="rendering === 'server'">
      First load or hard refresh is done on server side.
    </p>
    <p v-if="rendering === 'client'">Navigation is done on client side.</p>
    <article>
      <nuxt-content :document="page" />
    </article>

    <ul>
      <li v-for="link of page.toc" :key="link.id">
        <NuxtLink :to="`#${link.id}`">{{ link.text }}</NuxtLink>
      </li>
    </ul>

    <NuxtLink to="/about">About Page</NuxtLink>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  async asyncData(ctx) {
    const articles = await ctx
      .$content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .fetch()

    const page = await ctx.$content('articles', 'article-1').fetch()
    return {
      rendering: process.server ? 'server' : 'client',
      page,
      articles,
    }
  },
})
</script>

<style>
.icon.icon-link {
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
</style>
