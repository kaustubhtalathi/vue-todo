<template>
  <div class="container title">
    <div class="row">
        <div class="col">
          <h1>{{ title }}</h1>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AppTitle',
  props: {
    title: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
  text-align: center;
  color: #42b883;
  padding: 50px 0;
}
</style>
