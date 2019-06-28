<template>
  <div class="input-text-validation">
    <input class="input" />
    <div class="error-message">{{ errMsg }}</div>
  </div>
</template>

<script>
export default {
  name: 'InputTextValidation',
  props: {
    // TODO チェックパターンとチェックに必要なパラメータを追加
  },
  data: () => {
    return {
      errMsg: 'TODO 入力エラー時にエラーメッセージをセット'
    };
  },
  method: {
    // TODO チェックパターンごとにメソッドを追加
    // TODO input入力を検知してチェック処理を実施
  }
};
</script>

<style scoped>
.input-text-validation {
  text-align: left;
}

.error-message {
  color: red;
}
</style>
