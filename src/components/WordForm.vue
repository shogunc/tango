<template>
  <div class="container-fluid py-2" id="next">
    <div class="field font-weight-bold">{{ word.fbData.english }}</div>
    <div class="field pb-3">{{ word.fbData.englishExample }}</div>
    <div class="form-row align-items-center">
      <div class="col-auto">
        <input type="text" class="form-control mb-2" v-model="textInput">
      </div>
      <div class="col-auto">
        <button @click="checkResponse(textInput)" class="btn btn-primary mb-2" :disabled="textInput === ''">OK</button>
      </div>
    </div>  
  </div>
</template>

<script>
  export default {
    props: {
      word: Object
    },
    data () {
      return {
        textInput: ''
      }
    },
    methods: {
      checkResponse (response) {
        let result = this.word.fbData.kana === response || this.word.fbData.japanese.toUpperCase() === response.toUpperCase()
        this.textInput = ''
        this.$emit('result', result)
      },
    }
  }
</script>

<style type="text/css">
  #next {
    background-color: khaki;
    border-top: 2px solid darkkhaki;
    border-bottom: 2px solid darkkhaki;
  }
  .field {
    padding: 5px 0;
  }
</style>