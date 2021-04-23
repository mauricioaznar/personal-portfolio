<template>
  <v-row>
    <v-col cols="12">
      <prev-next :prev="prev" :next="next" directory="articles" />
    </v-col>
    <v-col cols="12">
      <table-of-content :toc="article.toc" />
    </v-col>
    <nuxt-content :document="article"></nuxt-content>
  </v-row>
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

        console.log(next)
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
  mounted() {
    Prism.highlightAll()
  },
})
</script>
