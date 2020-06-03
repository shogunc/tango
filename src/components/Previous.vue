<template>
  <div class="container-fluid mt-3 py-2" id="previous">
    <div class="field">{{ word.fbData.english }} - {{ word.fbData.kana }} ({{ word.fbData.japanese }})</div>
    <div class="field">{{ word.fbData.englishExample }}</div>
    <div class="field">{{ word.fbData.japaneseExample }}</div>
  </div>
</template>
<script>
  export default {
    props: {
      word: Object
    }
  }
</script>
<style type="text/css">
  #previous {
    background-color: blanchedalmond;
  }
  .field {
    padding: 5px 0;
  }
</style>