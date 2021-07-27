<template>
  <section class="tableofcontent">
    <h3>Table of Content</h3>
    <v-list v-model="selected">
      <v-treeview :items="items" hoverable transition>
        <template #label="{ item }">
          <v-list-item
            nuxt
            event=""
            :to="`#${item.id}`"
            @click.prevent.native="goTo"
          >
            {{ item.text }}
          </v-list-item>
        </template>
      </v-treeview>
    </v-list>
  </section>
</template>

<script>
export default {
  props: {
    toc: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selected: [],
      items: [],
    }
  },
  created() {
    let lowestNumber = 3
    this.items = this.toc
      .map((tocMapped) => {
        let hasReachedSameOrLowerDepth = false
        let hasReachedSameToc = false
        if (tocMapped.depth < lowestNumber) {
          lowestNumber = tocMapped.depth
        }
        return {
          ...tocMapped,
          children: this.toc.filter((tocFiltered) => {
            if (tocFiltered.id === tocMapped.id) {
              hasReachedSameToc = true
              return false
            }
            if (hasReachedSameOrLowerDepth) {
              return false
            }
            if (hasReachedSameToc) {
              if (tocMapped.depth < tocFiltered.depth) {
                return true
              } else {
                hasReachedSameOrLowerDepth = true
                return false
              }
            } else {
              return false
            }
          }),
        }
      })
      .filter((tocMapped) => {
        return tocMapped.depth === lowestNumber
      })
  },
  methods: {
    // eslint-disable-next-line @typescript-eslint/no-unused-vars
    resetScroll(entries, observer, isIntersecting) {
      if (window.location.hash !== '' && isIntersecting) {
        this.$refs['reset-hash'].$el.click()
      }
    },
    goTo(e) {
      const target = e.target.hash
      this.$vuetify.goTo(target, {
        duration: 1000,
        offset: 20,
        easing: 'easeInOutCubic',
      })
    },
  },
}
</script>
