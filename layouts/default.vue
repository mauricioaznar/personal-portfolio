<template>
  <v-app>
    <Keypress
      key-event="keydown"
      :modifiers="['altKey']"
      :key-code="67"
      @success="scrollTop"
    />
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router>
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn icon @click.stop="clipped = !clipped">
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-btn icon @click.stop="fixed = !fixed">
        <v-icon>mdi-minus</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-slide-y-reverse-transition>
        <v-btn
          v-show="showScrollButton"
          icon
          @click.stop="scrollTop"
          @keydown.alt.67="scrollTop"
        >
          <v-icon>mdi-arrow-up</v-icon>
        </v-btn>
      </v-slide-y-reverse-transition>
    </v-app-bar>
    <v-main>
      <v-container class="px-16 py-4">
        <nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="fixed"
      app
      justfiy
      class="d-flex justify-space-between"
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
      <v-slide-y-transition>
        <v-btn
          v-show="showScrollButton"
          transition="scroll-y-transition"
          text
          x-small
          @click="scrollTop"
          >Scroll to top
        </v-btn>
      </v-slide-y-transition>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  components: {
    Keypress: () => import('vue-keypress'),
  },
  data() {
    return {
      showScrollButton: true,
      clipped: false,
      drawer: false,
      fixed: false,

      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-script-text',
          title: 'Articles',
          to: '/articles',
        },
        {
          icon: 'mdi-format-list-numbered',
          title: 'Guides',
          to: '/guides',
        },
        {
          icon: 'mdi-code-braces',
          title: 'Snippets',
          to: '/snippets',
        },
      ],
      miniVariant: false,
      title: 'Vuetify.js',
    }
  },
  mounted() {
    const top = window.pageYOffset || document.documentElement.scrollTop
    if (top < 500) {
      this.showScrollButton = false
    }
    window.addEventListener('scroll', this.handleScroll)
  },
  destroyed() {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    handleScroll() {
      const top = window.pageYOffset || document.documentElement.scrollTop
      this.showScrollButton = top >= 500
      if (top <= 200) {
        window.location.hash = ''
      }
    },
    scrollTop() {
      this.$vuetify.goTo(0, {
        duration: 1000,
        offset: 0,
        easing: 'easeInOutCubic',
      })
    },
  },
}
</script>

<style>
.page-enter-active,
.page-leave-active,
.page-enter-active::before,
.page-enter-active::after,
.page-leave-active::before,
.page-leave-active::after {
  transition: transform 650ms cubic-bezier(0.17, 0.67, 0.54, 0.98);
}
.page-enter-active::before,
.page-leave-active::before {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
  z-index: 1000;
}
.page-enter-active::before {
  left: -417px;
}
.page-leave-active::before {
  right: -417px;
}
.page-enter-active::after,
.page-leave-active::after {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #fff;
}
.page-leave::before {
  transform: translate3d(calc(-100% - 417px), 0, 0) scaleX(-1);
}
.page-leave-to::before {
  transform: scaleX(-1);
}
.page-leave::after {
  transform: translate3d(calc(-100% - 417px), 0, 0);
}
.page-enter-to::before,
.page-enter-to::after {
  transform: translate3d(calc(100% + 417px), 0, 0);
}
</style>
