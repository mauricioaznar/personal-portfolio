<template>
  <v-container class="flex justify-between">
    <v-row>
      <v-col>
        <NuxtLink
          v-if="prev"
          :to="{ path: prev.path }"
          class="text-primary font-bold hover:underline"
        >
          {{ prev.title }}
        </NuxtLink>
        <span v-else>&nbsp;</span>
      </v-col>
      <v-col>
        <NuxtLink
          :to="{ path: '/' + directory }"
          class="text-primary font-bold hover:underline"
        >
          {{ directory }}
        </NuxtLink>
        <span>&nbsp;</span>
      </v-col>
      <v-col>
        <NuxtLink
          v-if="next"
          :to="{ path: next.path }"
          class="font-bold hover:underline"
        >
          {{ next.title }}
        </NuxtLink>
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
