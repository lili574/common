<template>
  <div id="app">
    <input-img v-model="foo"></input-img>
    <input-img v-model="bar"></input-img>
  </div>
</template>

<script>
import InputImg from './components/InputImg/input-img'
export default {
  name: 'App',
  data(){
    return {
      foo: 'https://iconfont.alicdn.com/t/1596002262649.jpg@100h_100w.jpg',
      bar: ''
    }
  },
  watch: {
  },
  components: {
    InputImg,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
