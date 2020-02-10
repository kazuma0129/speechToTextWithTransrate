<template>
  <v-container fluid>
    <v-col cols="12" md="7" lg="9" sm="9">
      <v-row>
        <v-textarea
          @change="transrate"
          v-model="inputText"
          :auto-grow="true"
          :clearable="true"
          :outlined="true"
          :rows="3"
          :row-height="30"
        >
        </v-textarea>
      </v-row>
      <v-row class="pa-4 mx-auto">
        <v-card>
          <v-card-text>
            {{ transratedText }}
          </v-card-text>
        </v-card>
      </v-row>
      <v-row class="pa-4 mx-auto">
        <v-card>
          <v-card-text>
            {{ reTransratedText }}
          </v-card-text>
        </v-card>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      inputText: '',
      transratedText: '',
      reTransratedText: '',
      transrateApi: `https://script.google.com/macros/s/AKfycby5NoNTXTgLP9y_2wVeQ0MMPAO0F4rQZsHiQfEhD3AONkFakYU/exec?`
    }
  },
  mounted() {},
  methods: {
    async getTransratedText(text, sourceLang, targetLang) {
      if (!text) {
        return null
      }
      const reqSource = `&source=${sourceLang}`
      const reqTarget = `&target=${targetLang}`
      const reqText = `&text=${text}`
      const url = this.transrateApi + reqTarget + reqSource + reqText
      let result = ''
      await this.$axios.get(url).then((res) => {
        result = res.data.text
      })
      return result
    },
    async transrate() {
      this.transratedText = null
      this.reTransratedText = null
      this.transratedText = await this.getTransratedText(
        this.inputText,
        'ja',
        'en'
      )
      this.reTransratedText = await this.getTransratedText(
        this.transratedText,
        'en',
        'ja'
      )
    }
  }
}
</script>
