<template>
  <div>
    <header-component 
      :title="'Add Word'"
    />
    <div class="container-fluid">
      <form>
        <div class="form-group">
          <label for="inputEnglish">Engelska</label>
          <input type="text" 
                 class="form-control" 
                 id="inputEnglish" 
                 v-model="word.english">
        </div>
        <div class="form-group">
          <label for="inputEnglishExample">Exempel (engelska)</label>
          <input type="text" 
                 class="form-control" 
                 id="inputEnglishExample" 
                 v-model="word.englishExample">
        </div>
        <div class="form-group">
          <label for="inputJapanese">Romaji</label>
          <input type="text" 
                 class="form-control" 
                 id="inputJapanese" 
                 v-model="word.japanese">
        </div>
        <div class="form-group">
          <label for="inputKana">Kana</label>
          <input type="text" 
                 class="form-control" 
                 id="inputKana" 
                 v-model="word.kana">
        </div>
        <div class="form-group">
          <label for="inputJapaneseExample">Exempel (japanska)</label>
          <input type="text" 
                 class="form-control" 
                 id="inputJapaneseExample" 
                 v-model="word.japaneseExample">
        </div>
      </form>
      <button type="submit" 
              class="btn btn-secondary" 
              @click=goHome>Tillbaka</button>
      <button type="submit" 
              class="btn btn-primary" 
              @click="submit"
              :disabled="!formValidated">Lägg till</button>
    </div>
  </div>
</template>

<script>
  import HeaderComponent from './components/Header.vue'
  export default {
  	components: {
  	  HeaderComponent
  	},
    data () {
      return {
        word: {
          english: '',
          englishExample: '',
          japanese: '',
          japaneseExample: '',
          kana: '',
          streak: 0,
          lastQuery: ''
        }
      }
    },
    computed: {
      formValidated: function () {
        return this.word.english !== '' && this.word.englishExample !== '' && this.word.kana !== '' && this.word.japanese !== '' && this.word.japaneseExample !== ''
      }
    },
    methods: {
      submit: function () {
        this.word.lastQuery = new Date()
        this.$http.post('words.json', this.word)
          .then(response => {
            console.log(response)
          }, error => {
            console.log(error)
          })
      },
      goHome: function () {
        this.$router.push('/')
      }
    }
  }
</script>