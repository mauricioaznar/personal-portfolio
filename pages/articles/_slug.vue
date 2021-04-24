<template>
  <v-container class="px-16">
    <v-row justify="center">
      <v-col cols="auto">
        <h1>
          {{ article.title }}
        </h1>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="10">
        <prev-next :prev="prev" :next="next" directory="articles" />
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="8">
        <table-of-content :toc="article.toc" />
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="8">
        <nuxt-content :document="article"></nuxt-content>
      </v-col>
    </v-row>
  </v-container>
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
          .only(['title', 'slug', 'path'])
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
