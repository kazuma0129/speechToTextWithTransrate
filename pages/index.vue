<template>
  <v-container fluid fill-height>
    <v-col col="12">
      <v-row align="center" justify="center">
        <textPresenter :font="font"></textPresenter>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */
/* eslint-disable no-unused-vars */
import textPresenter from '../components/textPresenter'

export default {
  name: 'Home',
  components: {
    textPresenter
  },
  data() {
    return {
      font: String
    }
  },
  watch: {},
  created() {},
  methods: {}
}
</script>
