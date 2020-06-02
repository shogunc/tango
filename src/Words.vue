<template>
	<div>
		<h1>Let's Tango!</h1>
		<router-link to="/add">Add word</router-link>
		<div id="previous" v-if="previousWord">
	      <p>{{ previousWord.fbData.english }}</p>
	      <p>{{ previousWord.fbData.japanese }}</p>
	      <p>{{ previousWord.fbData.englishExample }}</p>
	      <p>{{ previousWord.fbData.japaneseExample }}</p>
	      <p>{{ previousWord.correct ? 'Correct!' : 'Incorrect...' }}</p>
	    </div>
	    <div id="next" v-if="filteredWords.length > 0">
	      <p>{{ filteredWords[0].fbData.english }}</p>
	      <p>{{ filteredWords[0].fbData.englishExample }}</p>
	      <div class="form-row align-items-center">
		    <div class="col-auto">
			  <input type="text" class="form-control mb-2" v-model="textInput">
			</div>
			<div class="col-auto">
			  <button @click="checkResponse(textInput)" class="btn btn-primary mb-2">OK</button>
			</div>
		  </div>
	    </div>
	    <div id="done" v-else>
	      Done for now!
	    </div>
	</div>
</template>

<script>
	export default {
  data () {
    return {
      words: [],
      previousWord: null,
      textInput: ''
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
        	console.log(this.words);
        })
    },
    checkResponse (response) {
      this.updateWord(this.words[0], response);
      this.patchWord(this.words[0]);
      
      this.previousWord = this.words[0];
      this.previousWord.correct = this.isCorrect(this.words[0], response);
      this.words.shift();
      this.textInput = '';
    },
    isCorrect (word, response) {
      return word.fbData.kana === response || word.fbData.japanese.toUpperCase() === response.toUpperCase()
    },
    updateWord (word, response) {
      if (this.isCorrect(word, response)) {
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