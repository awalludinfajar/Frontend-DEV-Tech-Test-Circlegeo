<script>
import BaseMap from './components/BaseMap.vue';
import NavBar from './components/NavBar.vue';

export default {
    name: 'App',
    components: {
        NavBar,
        BaseMap
    }
}
</script>
<template>
    <div class="app">
        <NavBar/>
        <BaseMap />
    </div>
</template>
  
  
  <style>
  body {
    margin: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
      'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
      sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  code {
    font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
      monospace;
  }
  
  .app {
    text-align: center;
  }
  </style>