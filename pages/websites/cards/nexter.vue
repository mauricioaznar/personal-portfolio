<template>
  <v-card dark elevation="6" color="#101D2C" class="fill-height">
    <div
      class="d-flex flex-no-wrap justify-space-between align-stretch fill-height"
    >
      <div class="flex-grow-1 d-flex flex-column justify-space-between">
        <div>
          <v-card-title class="nexter-heading">Nexter</v-card-title>

          <v-card-subtitle
            ><b>Description: </b>A website about real state
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
              <li>Grid layout</li>
            </ul>
          </v-card-text>
        </div>

        <div>
          <v-divider dark />
          <v-card-actions>
            <a
              class="nexter-btn"
              href="https://nexter.mauaznar.com/"
              target="_blank"
            >
              Go to website
            </a>
          </v-card-actions>
        </div>
      </div>

      <div class="d-flex flex-wrap flex-column justify-center">
        <v-avatar class="ma-3 elevation-4" size="200" tile>
          <v-img src="/websites/img/nexter.png" position="top left"></v-img>
        </v-avatar>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'NexterCard',
}
</script>

<style scoped lang="scss">
.nexter-heading {
  color: #c69963;
}

.nexter-btn {
  background-color: #c69963;
  color: #fff;
  border: none;
  border-radius: 0;
  text-transform: uppercase;
  text-decoration: none;
  padding: 10px 20px;
  cursor: pointer;
  transition: all 0.2s;

  &:hover {
    background-color: #b28451;
  }
}
</style>
