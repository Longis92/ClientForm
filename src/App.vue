<template>
  <div id="app">
    <clientForm></clientForm>
  </div>
</template>

<script>
import clientForm from './components/clientForm'

export default {
  name: 'App',
  components: {
    clientForm
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto&display=swap');
@import './assets/sass/style.sass';
</style>