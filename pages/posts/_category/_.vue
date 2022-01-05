<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="auto" md="8">
        <v-container>
          <v-row justify="center">
            <v-col cols="auto">
              <h1>
                {{ posts.title }}
              </h1>
            </v-col>
          </v-row>
          <v-row justify="center">
            <v-col cols="1" class="text-center">
              <v-tooltip bottom>
                <template #activator="{ on, attrs }">
                  <v-btn icon small exact link nuxt :to="{ path: `/posts/${category}` }">
                    <v-icon color="primary" dark v-bind="attrs" v-on="on">
                      mdi-home
                    </v-icon>
                  </v-btn>
                </template>
                <span>{{ category }}</span>
              </v-tooltip>
              <span>&nbsp;</span>
            </v-col>
          </v-row>
          <v-row justify="center">
            <v-col>
              <table-of-content :toc="posts.toc" />
            </v-col>
          </v-row>
          <v-row justify="center">
            <v-col>
              <nuxt-content :document="posts"></nuxt-content>
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from 'vue'
import Prism from '@/plugins/prism'
import { IContentDocument } from '@nuxt/content/types/content'

export default Vue.extend({
  async asyncData(ctx) {
    let posts = null
    const prev = null
    const next = null

    const category = ctx.params.category
    try {
      const path = `/${category}/${ctx.params.pathMatch || 'index'}`

      const [art] = (await ctx
        .$content({ deep: true })
        .where({ path })
        .fetch()) as IContentDocument[]

      if (!art) {
        return ctx.error({ statusCode: 404, message: 'Post not found' })
      }

      posts = art
    } catch (e) {
      ctx.error({
        statusCode: 404,
        message: 'Page could not be found',
      })
    }

    return {
      category,
      posts,
      prev,
      next,
    }
  },
  mounted() {
    Prism.highlightAll()
  },
})
</script>


