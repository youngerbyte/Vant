<template>
  <div class="app">
    <button @click="click">Click</button>
    <collapse-box ref="collapse">
      <h1>content</h1>
      <h1>content</h1>
      <h1>content</h1>
      <h1>content</h1>
    </collapse-box>
  </div>
</template>

<script>
import CollapseBox from './collapse-box'

export default {
  name: 'App',
  components: {
    CollapseBox
  },
  methods: {
    click() {
      this.$refs.collapse.toggle();
    }
  }
}
</script>

<style>
.app {
    border: 1px solid red;
}
</style>