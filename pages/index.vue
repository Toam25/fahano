<template>
  <div class="container">
    {{ progress }}
    {{ result }}
    telema code : {{ telmaCode }} airtel code : {{ airtelCode }} orange code :
    {{ orangeCode }}
  </div>
</template>

<script lang="js">
import Tesseract from 'tesseract.js';
export default {
  name: 'IndexPage',
  components: {

  },
  data() {
    return {
      result: "",
      progress: 0,
      telmaCode: "",
      orangeCode: "",
      airtelCode: ''
    }
  },
  mounted() {
    this.extractTextFromImage('/aaa.png');
  },
  computed: {

  },
  methods: {

    async extractTextFromImage(imagePath) {
      let result = await Tesseract.recognize(
        imagePath,
        'fra', // Langue de l'image (français)
        {
          logger: m => {
            this.progress = Math.round(m.progress * 100);
            return m
          }
        } // Optionnel: afficher les logs
      );
      this.result = result.data.text; // Texte extrait de l'image
    },
    getTelmaCode(text) {
      const regex = /\b\d{14}\b/g;
      return text.match(regex)
    }
  },
  watch: {
    result(newValue, oldValue) {
      if (newValue.length > 0) {
        this.telmaCode = this.getTelmaCode(newValue)
      }
    }
  },
}
</script>
