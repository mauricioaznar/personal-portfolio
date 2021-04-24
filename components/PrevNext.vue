<template>
  <v-container class="flex justify-between">
    <v-row justify="space-between">
      <v-col cols="1" class="text-right">
        <v-tooltip v-if="prev" bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon small link nuxt :to="{ path: prev.path }">
              <v-icon color="primary" dark v-bind="attrs" v-on="on">
                mdi-arrow-left
              </v-icon>
            </v-btn>
          </template>
          <span>{{ prev.title }}</span>
        </v-tooltip>
        <span v-else>&nbsp;</span>
      </v-col>
      <v-col cols="1" class="text-center">
        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon small exact link nuxt :to="{ path: '/' + directory }">
              <v-icon color="primary" dark v-bind="attrs" v-on="on">
                mdi-home
              </v-icon>
            </v-btn>
          </template>
          <span>{{ directory }}</span>
        </v-tooltip>
        <span>&nbsp;</span>
      </v-col>
      <v-col cols="1">
        <v-tooltip v-if="next" bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn icon small link nuxt :to="{ path: next.path }">
              <v-icon color="primary" dark v-bind="attrs" v-on="on">
                mdi-arrow-right
              </v-icon>
            </v-btn>
          </template>
          <span>{{ next.title }}</span>
        </v-tooltip>
        <span v-else>&nbsp;</span>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    prev: {
      type: Object,
      default: () => null,
    },
    next: {
      type: Object,
      default: () => null,
    },
    directory: {
      type: String,
      default: () => {
        return ''
      },
    },
  },
}
</script>
