<template>
  <div id="app">
    <!--
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    -->
    <h1>input-text-validation</h1>
    <InputTextValidation
      class="input-text-validation"
      :required="true"
      :min="5"
      :max="10"
    />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue';
import InputTextValidation from './components/InputTextValidation.vue';

export default {
  name: 'app',
  components: {
    // HelloWorld
    InputTextValidation
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<style scoped>
.input-text-validation {
  padding: 0 40px;
}
</style>
