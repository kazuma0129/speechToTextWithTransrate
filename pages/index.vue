<template>
  <v-container fluid fill-height>
    <v-col col="12">
      <v-row align="center" justify="center">
        <speechToText />
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */
/* eslint-disable no-unused-vars */
import speechToText from '~/components/speechToText'

export default {
  name: 'Index',
  components: {
    speechToText
  },
  data() {
    return {}
  },
  watch: {},
  created() {},
  methods: {}
}
</script>
