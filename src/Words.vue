<template>
	<div>
		<h1>Let's Tango!</h1>
		<router-link to="/add">Add word</router-link>
		<div id="previous" v-if="previousWord">
	      <p>{{ previousWord.english }}</p>
	      <p>{{ previousWord.japanese }}</p>
	      <p>{{ previousWord.englishExample }}</p>
	      <p>{{ previousWord.japaneseExample }}</p>
	      <p>{{ previousWord.correct ? 'Correct!' : 'Incorrect...' }}</p>
	    </div>
	    <div id="next" v-if="words.length > 0">
	      <p>{{ words[0].english }}</p>
	      <p>{{ words[0].englishExample }}</p>
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
    this.words = this.fetchWords();
  },
  methods: {
    fetchWords () {
      return [{
        english: 'Direct flight',
        englishExample: 'Direct flights between New York and Tokyo commenced recently.',
        japanese: '直行便',
        japaneseExample: 'ニューヨーク・東京間の直行便が最近開始された。',
        romaji: 'chokkoubin',
        streak: 0,
        lastQuery: new Date()
      },
      {
        english: 'Postponement',
        englishExample: 'The baseball game was put off till next Sunday.',
        japanese: '延期',
        japaneseExample: '野球の試合は来週の日曜日まで延期された。',
        romaji: 'enki',
        streak: 0,
        lastQuery: new Date()
      }]
    },
    checkResponse (response) {
      const correct = this.words[0].japanese === response || this.words[0].romaji === response;
      if (correct) {
        this.words[0].streak++;
      } else {
        this.words[0].streak = 0;
      }
      this.words[0].lastQuery = new Date();
      this.previousWord = this.words[0];
      this.previousWord.correct = correct;
      this.updateWord(this.words[0], correct);
      this.words.shift();
      this.textInput = '';
    },
    updateWord (word, correct) {
      console.log('Updating word: ' + word.english);
    }
  }
}
</script>