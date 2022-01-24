<template>
  <WeatherWidget msg="How 'bout that weather?"/>
</template>

<script>
  import WeatherWidget from './components/WeatherWidget.vue'

  export default {
    name: 'App',
    components: {
      WeatherWidget
    }
  }
</script>

<style>
  body {
    font-family: Helvetica, Arial, sans-serif;
    margin: 0;
  }
</style>
