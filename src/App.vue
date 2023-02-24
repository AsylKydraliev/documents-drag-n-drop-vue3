<template>
  <div class="container">
    <DocumentsView/>
  </div>
</template>

<script>
import DocumentsView from "@/views/DocumentsView.vue";

export default {
  name: 'App',
  components: {DocumentsView},
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
.container {
  margin: 0 auto;
  padding: 0 15px;
  max-width: 1200px;
}
</style>
