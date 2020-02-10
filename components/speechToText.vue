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

      <v-row justify="center">
        <h1>{{ reTransratedText }}</h1>
      </v-row>
    </v-col>
  </v-container>
</template>

<script>
export default {
  name: 'SpeechToText',
  data() {
    return {
      state: Boolean,

      text: null,
      transratedText: null,
      reTransratedText: null,

      /* eslint-disable-next-line */
      recognition: new webkitSpeechRecognition(),

      sourceLang: 'ja',
      targetLang: 'en',
      source: null,
      target: null,
      transrateApi: `https://script.google.com/macros/s/AKfycby5NoNTXTgLP9y_2wVeQ0MMPAO0F4rQZsHiQfEhD3AONkFakYU/exec?`
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
  async mounted() {},
  methods: {
    async getTransratedText(text, sourceLang, targetLang) {
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
    speechRecognize() {
      this.recognition.onspeechend = async () => {
        if (this.text) {
          this.transratedText = await this.getTransratedText(
            this.text,
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
