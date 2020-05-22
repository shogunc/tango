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
	    <div id="next" v-if="words.length > 0">
	      <p>{{ words[0].fbData.english }}</p>
	      <p>{{ words[0].fbData.englishExample }}</p>
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
      // return [{
      //   english: 'Direct flight',
      //   englishExample: 'Direct flights between New York and Tokyo commenced recently.',
      //   japanese: '直行便',
      //   japaneseExample: 'ニューヨーク・東京間の直行便が最近開始された。',
      //   romaji: 'chokkoubin',
      //   streak: 0,
      //   lastQuery: new Date()
      // },
      // {
      //   english: 'Postponement',
      //   englishExample: 'The baseball game was put off till next Sunday.',
      //   japanese: '延期',
      //   japaneseExample: '野球の試合は来週の日曜日まで延期された。',
      //   romaji: 'enki',
      //   streak: 0,
      //   lastQuery: new Date()
      // }]
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
    }
  }
}
</script>