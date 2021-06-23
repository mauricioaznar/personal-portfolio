<template>
  <v-card dark elevation="6">
    <div class="d-flex flex-no-wrap justify-space-between align-stretch">
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div style="background-color: #333333">
          <v-card-title class="trillo-heading"> Trillo </v-card-title>

          <v-card-subtitle
            ><b>Description: </b>A website about hotel booking
            <br />
            <i
              >Credits to: Jonas Schmedtmann (<a
                href="https://www.udemy.com/course/advanced-css-and-sass/"
                >Advanced CSS and Sass Course</a
              >)
            </i>
          </v-card-subtitle>

          <v-card-text>
            <ul>
              <li>Animations</li>
              <li>Flex layout</li>
            </ul>
          </v-card-text>
        </div>

        <v-divider dark />

        <v-card-actions style="background-color: #333333">
          <a
            class="trillo-btn"
            href="https://trillo.mauaznar.com/"
            target="_blank"
          >
            <span class="trillo-btn__visible">Go to website</span>
            <span class="trillo-btn__invisible">Now!</span>
          </a>
        </v-card-actions>
      </div>

      <div
        class="d-flex flex-wrap flex-column justify-center"
        style="background-color: #ffffff"
      >
        <v-avatar class="ma-3 elevation-4" size="200" tile>
          <v-img src="/websites/img/trillo.png"></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'TrilloCard',
}
</script>

<style scoped lang="scss">
.trillo-heading {
  color: #eb2f64;
}
@keyframes pulsate {
  0% {
    transform: scale(1);
    box-shadow: none;
  }

  50% {
    transform: scale(1.05);
    box-shadow: 0 1rem 4rem rgba(0, 0, 0, 0.25);
  }

  100% {
    transform: scale(1);
    box-shadow: none;
  }
}

.trillo-btn {
  font-weight: 300;
  text-transform: uppercase;
  border: none;
  border-radius: 100px;
  background-image: linear-gradient(to right, #eb2f64, #ba265d);
  color: #fff;
  position: relative;
  overflow: hidden;
  cursor: pointer;

  & > * {
    display: inline-block;
    height: 100%;
    transition: all 0.2s;
    width: 100%;
    text-align: center;
    word-break: keep-all;
  }

  &__visible {
    padding: 10px 20px;
  }

  &__invisible {
    position: absolute;
    padding: 10px 20px;
    left: 0;
    top: -100%;
  }

  &:hover {
    background-image: linear-gradient(to left, #eb2f64, #ba265d);
  }

  &:hover &__invisible {
    top: 0;
  }

  &:hover &__visible {
    transform: translateY(100%);
  }

  &:focus {
    outline: none;
    animation: pulsate 1s infinite;
  }
}
</style>
