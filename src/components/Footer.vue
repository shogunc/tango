<template>
  <div class="container-fluid py-2">
    <div v-if="numberOfWords > 0">
      There are {{ numberOfWords }} words in the queue.
    </div>
    <div v-else>
      The queue is empty, how about adding a new word?
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