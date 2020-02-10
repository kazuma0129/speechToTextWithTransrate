<template>
  <v-app>
    <v-content>
      <!-- <nuxt /> -->
      <router-view />
    </v-content>
    <app-bottom-navigation-bar />
  </v-app>
</template>

<script>
import AppBottomNavigationBar from '~/components/AppBottomNavigationBar'

export default {
  components: {
    AppBottomNavigationBar
  }
}
</script>

<style>
a {
  color: inherit;
  text-decoration: none;
}
a:link {
  color: inherit;
  text-decoration: none;
}
a:hover {
  color: inherit;
  text-decoration: none;
}
a:visited {
  color: inherit;
  text-decoration: none;
}
a:active {
  color: inherit;
  text-decoration: none;
}
</style>
