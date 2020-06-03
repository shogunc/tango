<template>
  <div>
    <header-component 
      :title="'Let\'s Tango!'"
      :link="'/add'"
      :link-label="'Add word'"
    />
    <word-form 
        :word="filteredWords[0]" 
        v-if="filteredWords.length > 0"
        v-on:result=checkResponse />
    <previous 
      v-if=previousWord 
      :word="previousWord" />
    <footer-component :numberOfWords="filteredWords.length" />
  </div>
</template>

<script>
  import WordForm from './components/WordForm.vue'
  import HeaderComponent from './components/Header.vue'
  import FooterComponent from './components/Footer.vue'
  import Previous from './components/Previous.vue'
  export default {
    components: {
      WordForm, 
      HeaderComponent, 
      FooterComponent, 
      Previous
    },
    data () {
      return {
        words: [],
        previousWord: null,
      }
    },
    computed: {
      filteredWords () {
        const second = 1000;
        const minute = 60 * second;
        const hour = 60 * minute;
        const day = 24 * hour;

        return this.words.filter(word => {
          switch (word.fbData.streak) {
            case 0: 
              return true;
            case 1: 
              return this.timeSinceLastQuery(word) > 1 * minute;
            case 2: 
              return this.timeSinceLastQuery(word) > 2 * minute;
            case 3: 
              return this.timeSinceLastQuery(word) > 3 * minute;
            case 4: 
              return this.timeSinceLastQuery(word) > 4 * minute;
            case 5: 
              return this.timeSinceLastQuery(word) > 5 * minute;
            case 6: 
              return this.timeSinceLastQuery(word) > 6 * minute;
            case 7: 
              return this.timeSinceLastQuery(word) > 7 * minute;
            default:
              return this.timeSinceLastQuery(word) > 8 * minute;
          }
        })
      }
    },
    mounted () {
      this.fetchWords();
    },
    methods: {
      fetchWords () {
        this.$http.get('words.json')
          .then(response => {
            return response.json();
          })
          .then(data => {
            const resultArray = [];
            for (let key in data) {
              resultArray.push({
                fbKey: key,
                fbData: data[key]	
              });
            }
            this.words = resultArray;
          })
      },
      checkResponse (isCorrect) {
        this.updateWord(this.words[0], isCorrect);
        this.patchWord(this.words[0]);
        
        this.previousWord = this.words[0];
        this.previousWord.correct = isCorrect;
        this.words.shift();
        this.textInput = '';
      },
      updateWord (word, isCorrect) {
        if (isCorrect) {
          word.fbData.streak++;
        } else {
          word.fbData.streak = 0;
        }
        word.fbData.lastQuery = new Date();
      },
      patchWord (word) {
        this.$http.patch('words/' + word.fbKey + '/.json', word.fbData)
          .then(response => {
            return response.json();
          }, error => {
            console.log(error)
          })
      },
      timeSinceLastQuery (word) {
        return Date.now() - Date.parse(word.fbData.lastQuery);
      }
    }
  }
</script>
<style type="text/css">
  
</style>