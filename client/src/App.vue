<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-md-6 offset-md-3">
          <Panel />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Panel from './components/Panel.vue'

export default {
  name: 'app',
  components: {
    Panel
  }
}
</script>

<style>
</style>
