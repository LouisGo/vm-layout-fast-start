<template>
  <div id="app" class="app ignore">
    <div aspectratio ratio-16-9>
      <img aspectratio-content src="./assets/logo.png">
    </div>
    <router-view/>
    <div style="border: 1px solid #ccc;">125</div>
    <div class="test">
      21412
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App'
  }

</script>

<style>
  .app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .ignore {
    margin-top: 60px;
  }

  [aspectratio] {
    position: relative;
  }

  [aspectratio]::before {
    content: '';
    display: block;
    width: 1px;
    margin-left: -1px;
    height: 000%  }

  [aspectratio-content] {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
  }

  [ratio-1-1] {
    aspect-ratio: '1:1';
  }

  [ratio-16-9] {
    aspect-ratio: '16:9';
  }

  [ratio-4-3] {
    aspect-ratio: '4:3';
  }

  img {
    content: normal !important;
  }

  .test {
    position: relative;
    width: 20vw;
    height: 10vw;
    margin-top: 20px;
    background: #fff;
  }

  .test:after {
    content: '';
    position: absolute;
    left: 0;
    background: #ddd;
    width: 1px;
    height: 100%;
    -webkit-transform: scaleY(0.5);
    transform: scaleY(0.5);
    -webkit-transform-origin: 0 0;
    transform-origin: 0 0;

  }

</style>
