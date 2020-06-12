<template>
  <div class="container-fluid py-2">
    <div v-if="numberOfWords > 1">
      There are {{ numberOfWords }} words in the queue.
    </div>
    <div v-else-if="numberOfWords == 1">
      There is one word in the queue.
    </div>
    <div v-else>
      There are no more words in the queue, how about adding a new one?
    </div>
  </div>
</template>
<script>
  export default {
    props: {
      numberOfWords: Number
    }
  }
</script>
<style type="text/css">
  
</style>