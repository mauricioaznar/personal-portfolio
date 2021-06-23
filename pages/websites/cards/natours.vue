<template>
  <v-card class="natours-card" dark>
    <div class="d-flex flex-no-wrap justify-space-between align-stretch">
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title class="natours-heading"> Natours </v-card-title>

          <v-card-subtitle
            ><b>Description: </b>A website about tours on the nature
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
              <li>Float layout</li>
              <li>Radio an checkbox hack</li>
            </ul>
          </v-card-text>
        </div>

        <v-divider />

        <v-card-actions>
          <a
            class="natours-btn natours-btn--green natours-btn--animated"
            href="https://natours.mauaznar.com/"
            target="_blank"
          >
            Go to website
          </a>
        </v-card-actions>
      </div>

      <div class="d-flex flex-wrap flex-column justify-center">
        <v-avatar class="ma-3 ml-0 elevation-4" size="200" tile>
          <v-img src="/websites/img/natours.png"></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'NatoursCard',
}
</script>

<style scoped lang="scss">
.natours-heading {
  hyphens: auto;
  font-size: 20px;
  text-transform: uppercase;
  letter-spacing: 10px;
  font-weight: 400;
  display: inline-block;
  background-image: linear-gradient(to right, #fff, #ccc);
  -webkit-background-clip: text;
  color: transparent;
  transition: all 0.2s;

  &:hover {
    transform: skewY(2deg) skewX(15deg) scale(1.1);
    text-shadow: 0 6px 12px rgba(#000, 0.3);
  }
}

.natours-card {
  background-image: linear-gradient(to right bottom, #55c57a, #28b485);
}

@keyframes moveInBottom {
  0% {
    opacity: 0;
    transform: translateY(3rem);
  }

  100% {
    opacity: 1;
    transform: translate(0);
  }
}

.natours-btn {
  &,
  &:link,
  &:visited {
    text-transform: uppercase;
    text-decoration: none;
    padding: 7.5px 20px;
    display: inline-block;
    border-radius: 100px;
    transition: all 0.2s;
    position: relative;
    font-size: 16px;

    border: none;
    cursor: pointer;
  }

  &:hover {
    transform: translateY(-0.3rem);
    box-shadow: 0 1rem 2rem rgba(#000, 0.2);

    &::after {
      transform: scaleX(1.4) scaleY(1.6);
      opacity: 0;
    }
  }

  &:active,
  &:focus {
    outline: none;
    transform: translateY(-0.1rem);
    box-shadow: 0 0.5rem 1rem rgba(#000, 0.2);
  }

  &--green {
    background-color: #7ed56f;
    color: #fff;

    &::after {
      background-color: #7ed56f;
    }
  }

  &::after {
    content: '';
    display: inline-block;
    height: 100%;
    width: 100%;
    border-radius: 10rem;
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    transition: all 0.4s;
  }

  &--animated {
    animation: moveInBottom 0.5s ease-out 0.75s;
    animation-fill-mode: backwards;
  }
}
</style>
