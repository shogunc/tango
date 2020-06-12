<template>
  <div class="container-fluid mt-3 py-2" id="previous">
  	<div class="splash correct" v-if="word.correct">Streak: {{ word.fbData.streak }}</div>
  	<div class="splash incorrect" v-else>Incorrect</div>
    <div class="field font-weight-bold">{{ word.fbData.english }} - {{ word.fbData.kana }} ({{ word.fbData.japanese }})</div>
    <div class="field">{{ word.fbData.englishExample }}</div>
    <div class="field">{{ word.fbData.japaneseExample }}</div>
  </div>
</template>
<script>
  export default {
    props: {
      word: Object
    },
    mounted () {
      console.log(this.word);
    }
  }
</script>
<style type="text/css">
  #previous {
    background-color: lightsalmon;
    border-top: 2px solid indianred;
    border-bottom: 2px solid indianred;
    position: relative;
  }
  .field {
    padding: 5px 0;
  }
  .splash {
    height: 20px;
    width: 75px;
    border-radius: 10px;
    position: absolute;
    top: -10px;
    right: 15px;
    font-size: 10px;
    font-weight: bold;
    text-align: center;
    vertical-align: middle;
  }
  .correct {
    background-color: lightgreen;
    border: 2px solid green;
  }
  .incorrect {
    background-color: tomato;
    border: 2px solid red;
  }
</style>