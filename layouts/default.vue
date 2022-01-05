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
      :mini-variant="$vuetify.breakpoint.mdAndUp ? miniVariant : true"
      :clipped="false"
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
    <v-app-bar :clipped-left="false" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-slide-y-transition>
        <v-btn
          v-if="$vuetify.breakpoint.mdAndUp && drawer"
          transition="scroll-y-transition"
          icon
          @click.stop="miniVariant = !miniVariant"
        >
          <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
        </v-btn>
      </v-slide-y-transition>
      <v-spacer></v-spacer>
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
      <v-container class="py-4" fluid>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="false"
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
      drawer: false,
      items: [
        {
          icon: 'mdi-home',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-apps',
          title: 'App demos',
          to: '/demos',
        },
        {
          icon: 'mdi-web',
          title: 'Websites',
          to: '/websites',
        },
        {
          icon: 'mdi-script-text',
          title: 'Articles',
          to: '/posts/articles',
        },
               {
          icon: 'mdi-script-text',
          title: 'Snippets',
          to: '/posts/snippets',
        },
        {
          icon: 'mdi-palette',
          title: 'Art',
          to: '/art',
        },
        {
          icon: 'mdi-flask-outline',
          title: 'Tests',
          to: '/tests',
        },
      ],
      miniVariant: false,
      title: 'Personal portfolio - Mauricio Aznar',
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
  z-index: 5;
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
  z-index: 5;
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
