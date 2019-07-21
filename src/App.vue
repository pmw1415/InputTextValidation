<template>
  <div id="app">
    <!--
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    -->
    <h1>input-text-validation</h1>
    <div class="validation">
      <div class="label">
        必須チェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :required="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        文字数チェック(min/max未入力で指定なし)
      </div>
      <div class="param">
        <div class="param-min">
          <span class="label">min:</span>
          <input
            v-model="min"
            class="value"
            type="number"
            step="1"
            min="0"
          />
        </div>
        <div class="param-max">
          <span class="label">max:</span>
          <input
            v-model="max"
            class="value"
            type="number"
            step="1"
            min="0"
          />
        </div>
      </div>
      <InputTextValidation
        class="input-text-validation"
        :min="min"
        :max="max"
      />
    </div>

    <div class="validation">
      <div class="label">
        数値チェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        type="number"
        :numeric="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        半角英数チェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :alphaNum="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        全角チェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :zen="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        電話番号チェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :tel="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        メールアドレスチェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :mail="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        URLチェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :url="true"
      />
    </div>

    <div class="validation">
      <div class="label">
        日付チェック
      </div>
      <InputTextValidation
        class="input-text-validation"
        :date="true"
      />
    </div>
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
  },
  data() {
    return {
      min: 5,
      max: 10
    };
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
.validation {
  padding: 20px 40px;
  text-align: left;
}

.param {
  display: flex;
}

.param-min,
.param-max {
  display: flex;
  width: 100px;
}
.param
.param-min .label,
.param-max .label {
  width: 40px;
}
.param-min .value,
.param-max .value {
  width: 40px;
}
</style>
