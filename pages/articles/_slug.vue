<template>
  <div>
    <prev-next :prev="prev" :next="next" directory="articles" />
    <nuxt-content :document="article"></nuxt-content>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { IContentDocument } from '@nuxt/content/types/content'

export default Vue.extend({
  async asyncData(ctx) {
    const article = await ctx.$content('articles', ctx.params.slug).fetch()

    let prev = null
    let next = null

    if (ctx.params.slug) {
      const [prevArticle = null, nextArticle = null] = (await ctx
        .$content('articles')
        .only(['title', 'slug'])
        .sortBy('createdAt', 'asc')
        .surround(ctx.params.slug)
        .fetch()) as Array<IContentDocument>

      prev = prevArticle
      next = nextArticle
    }

    return {
      article,
      prev,
      next,
    }
  },
})
</script>
