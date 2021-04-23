<template>
  <div>
    <prev-next :prev="prev" :next="next" directory="articles" />
    <table-of-content :toc="article.toc" />
    <nuxt-content :document="article"></nuxt-content>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { IContentDocument } from '@nuxt/content/types/content'
import Prism from '@/plugins/prism'

export default Vue.extend({
  async asyncData(ctx) {
    let article = null
    let prev = null
    let next = null

    if (ctx.params.slug) {
      try {
        article = await ctx.$content('articles', ctx.params.slug).fetch()
        const [prevArticle = null, nextArticle = null] = (await ctx
          .$content('articles')
          .only(['title', 'slug'])
          .sortBy('createdAt', 'asc')
          .surround(ctx.params.slug)
          .fetch()) as Array<IContentDocument>

        prev = prevArticle
        next = nextArticle
      } catch (e) {
        ctx.error({
          statusCode: 404,
          message: 'Page could not be found',
        })
      }
    }

    return {
      article,
      prev,
      next,
    }
  },
  created() {
    console.log('asdfasd')
  },
  mounted() {
    Prism.highlightAll()
  },
})
</script>
