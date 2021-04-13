<template>
  <div id="app">
    <main>
      <Weather/>
    </main>
  </div>
</template>

<script>
import Weather from './components/Weather'

export default {
  name: 'App',
  components: {
    Weather
  }
}
</script>

<style>
body {
  background-image: url('./assets/weather.jpg');
  background-size: cover;
}

main {
  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
</style>