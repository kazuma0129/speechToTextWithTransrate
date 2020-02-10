<template>
  <v-bottom-navigation v-model="bottomNav" fixed app>
    <v-btn value="index">
      <span>Speech</span>
      <nuxt-link to="/"><v-icon>mdi-voice</v-icon></nuxt-link>
    </v-btn>

    <v-btn value="write">
      <span>Write</span>
      <nuxt-link to="/write"><v-icon>mdi-account</v-icon></nuxt-link>
    </v-btn>
  </v-bottom-navigation>
</template>

<script>
export default {
  name: 'AppBottomNavigationBar',
  data() {
    return {
      bottomNav: 'index'
    }
  }
}
</script>
