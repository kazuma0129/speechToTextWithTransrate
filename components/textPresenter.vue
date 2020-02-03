<template>
  <v-container fluid>
    <v-col>
      <v-row justify="center">
        <h1>{{ text }}</h1>
      </v-row>

      <v-row justify="center">
        <v-btn
          v-show="!state"
          @click="startSpeech"
          color="primary"
          outlined
          large
          >️️️⚡️go⚡️</v-btn
        >
        <v-btn v-show="state" @click="stopSpeech" large outlined color="warning"
          >✋stop✋</v-btn
        >
      </v-row>

      <v-row justify="center">
        <h1>{{ transratedText }}</h1>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
export default {
  name: 'TextPresenter',
  data() {
    return {
      state: Boolean,

      text: null,
      transratedText: null,

      /* eslint-disable-next-line */
      recognition: new webkitSpeechRecognition(),

      sourceLang: 'ja',
      targetLang: 'en',
      source: null,
      target: null,
      transrateApi: null
    }
  },
  watch: {
    state() {
      if (!this.state) {
        this.stopSpeech()
      }
    }
  },
  created() {
    this.speechRecognize()
    this.state = false
  },
  mounted() {
    this.source = `source=${this.sourceLang}`
    this.target = `target=${this.targetLang}`
    this.transrateApi = `https://script.google.com/macros/s/AKfycby5NoNTXTgLP9y_2wVeQ0MMPAO0F4rQZsHiQfEhD3AONkFakYU/exec?${this.source}&${this.target}&`
  },
  methods: {
    async getTransratedText(text) {
      const url = this.transrateApi + 'text=' + text
      await this.$axios.get(url).then((res) => {
        this.transratedText = res.data.text
      })
    },
    speechRecognize() {
      this.recognition.onspeechend = () => {
        this.getTransratedText(this.text)
      }
      this.recognition.onend = () => {
        if (this.state) {
          this.startSpeech()
        } else {
          this.stopSpeech()
        }
      }
      this.recognition.onresult = (event) => {
        let interimTranscript = ''
        let finalTranscript = ''
        for (let i = event.resultIndex; i < event.results.length; i++) {
          const transcript = event.results[i][0].transcript
          if (event.results[i].isFinal) {
            finalTranscript += transcript
          } else {
            interimTranscript = transcript
          }
          this.text = finalTranscript + interimTranscript
        }
      }
    },
    startSpeech() {
      this.recognition.lang = 'ja-JP'
      this.recognition.interimResults = true
      this.state = true
      this.recognition.start()
    },
    stopSpeech() {
      this.state = false
      this.recognition.stop()
    }
  }
}
</script>
