<template>
  <v-container fluid fill-height>
    <v-col col="12" align="center" justify="center">
      <v-row>
        <transrate />
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
/* eslint-disable no-console */
import transrate from '~/components/transrate'

export default {
  name: 'Write',
  components: {
    transrate
  },
  data() {
    return {}
  },
  watch: {},
  created() {},
  methods: {}
}
</script>
