<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="auto" md="8">
        <v-container>
          <v-row justify="center">
            <v-col cols="auto">
              <h1>
                {{ snippet.title }}
              </h1>
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <prev-next :prev="prev" :next="next" directory="snippets" />
            </v-col>
          </v-row>
          <v-row justify="center">
            <v-col cols="12">
              <table-of-content :toc="snippet.toc" />
            </v-col>
          </v-row>
          <v-row justify="center">
            <v-col>
              <nuxt-content :document="snippet"></nuxt-content>
            </v-col>
          </v-row>
        </v-container>
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
    let snippet = null
    let prev = null
    let next = null

    if (ctx.params.slug) {
      try {
        snippet = await ctx.$content('snippets', ctx.params.slug).fetch()
        const [prevArticle = null, nextArticle = null] = (await ctx
          .$content('snippets')
          .only(['title', 'slug', 'path'])
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
      snippet,
      prev,
      next,
    }
  },
  mounted() {
    Prism.highlightAll()
  },
})
</script>
