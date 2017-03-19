<template>
  <div>
    <Navigation></Navigation>

    <div class="container">
      <div id="app">
        <hello></hello>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello';
import Navigation from './components/Navigation';

import bootstrap from 'bootstrap/dist/css/bootstrap.css';

import bootstrapjs from 'bootstrap/dist/js/bootstrap.js';

export default {
  name: 'app',
  components: {
    Hello,
    Navigation
  }
}
</script>

<style>
#app {
  
}
</style>
