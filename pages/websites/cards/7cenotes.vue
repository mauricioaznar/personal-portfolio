<template>
  <v-card class="siete-card fill-height" dark>
    <div
      class="d-flex flex-no-wrap justify-space-between align-stretch fill-height"
    >
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title class="siete-heading"> 7 cenotes </v-card-title>

          <v-card-subtitle
            ><b>Description: </b>A website about a tourism company
            <br />
          </v-card-subtitle>

          <v-card-text>
            <ul>
              <li>Animations</li>
              <li>Parallax</li>
              <li>Gallery</li>
              <li>Embedded video</li>
            </ul>
          </v-card-text>
        </div>
        <div>
          <v-divider />
          <v-card-actions>
            <a
              class="siete-btn siete-btn--green siete-btn--animated"
              href="https://www.los7cenotes.com/"
              target="_blank"
            >
              Go to website
            </a>
          </v-card-actions>
        </div>
      </div>

      <div
        class="d-flex flex-wrap flex-column justify-center stack"
        @mouseenter="onmouseenter"
        @mouseleave="onmouseleave"
      >
        <v-avatar
          class="ma-3 ml-0 elevation-4 siete-img-1 photo photo-decoration"
          size="200"
          tile
        >
          <v-img src="/websites/img/siete-1.png"></v-img>
        </v-avatar>
        <v-avatar
          :class="{ active: stackHovered }"
          class="ma-3 ml-0 elevation-4 siete-img-2 photo photo-decoration"
          size="200"
          tile
        >
          <v-img src="/websites/img/siete-2.png"></v-img>
        </v-avatar>
        <v-avatar
          :class="{ active: stackHovered }"
          class="ma-3 ml-0 elevation-4 siete-img-3 photo photo-decoration"
          size="200"
          tile
        >
          <v-img src="/websites/img/siete-3.png"></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'SieteCenotesCard',
  data() {
    return {
      stackHovered: false,
    }
  },
  methods: {
    onmouseenter() {
      this.stackHovered = true
    },
    onmouseleave() {
      this.stackHovered = false
    },
  },
}
</script>

<style scoped lang="scss">
.siete-heading {
  text-transform: uppercase;
  text-shadow: 2px 2px 4px #000;
  color: #eeeff7;
  font-weight: 500;
  line-height: 1.1;
}

.siete-card {
  background: #31353d;
}

.siete-btn {
  &,
  &:link,
  &:visited {
    text-transform: uppercase;
    text-decoration: none;
    padding: 7.5px 20px;
    display: inline-block;
    transition: all 0.2s;
    position: relative;
    font-size: 16px;
    background-color: #fff;
    border: none;
    cursor: pointer;
    color: #31353d;
  }

  &:hover {
    transform: translateY(-0.3rem);
    box-shadow: 0 1rem 2rem rgba(#000, 0.2);
  }

  &:active,
  &:focus {
    outline: none;
    transform: translateY(-0.1rem);
    box-shadow: 0 0.5rem 1rem rgba(#000, 0.2);
  }
}

.siete-img-1 {
  position: relative;
}

.siete-img-2 {
  position: absolute;
}

.siete-img-3 {
  position: absolute;
}

.stack {
  position: relative;
  width: 300px;
  height: 300px;
}

.photo {
  border: 3px solid white;
  box-sizing: border-box;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
  transition: all 0.2s linear;
  &:hover {
    box-shadow: 0 0 20px #fff;
  }
}

@mixin translateAndRotate($x, $y, $deg) {
  transform: translate($x, $y) rotate($deg);
}

@mixin rotate($value) {
  transform: rotate($value);
}

@mixin translate($x, $y) {
  transform: translate($x, $y);
}

.photo-decoration {
  &:nth-child(1) {
    z-index: 4;
  }
  &:nth-child(2) {
    z-index: 2;
    @include translateAndRotate(10%, 3%, 10deg);
    &.active.photo {
      @include translateAndRotate(70%, 15%, 20deg);
    }
  }
  &:nth-child(3) {
    z-index: 3;
    @include translateAndRotate(0%, 3%, -5deg);
    &.active.photo {
      @include translateAndRotate(-60%, 15%, -20deg);
    }
  }
}
</style>
