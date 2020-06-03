<template>
  <div class="container-fluid py-2">
    <div class="row">
      <div class="col-8">
        <h2>{{ title }}</h2>
      </div>
      <div class="col link" v-if="link && linkLabel">
        <router-link :to="link">{{ linkLabel }}</router-link>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    props: {
      title: String,
      link: String,
      linkLabel: String
    }
  }
</script>
<style type="text/css">
  .link {
    margin: auto;
    text-align: right;
  }
</style>