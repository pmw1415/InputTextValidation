<template>
  <div class="input-text-validation">
    <input
      :class="{ error: hasInputError }"
      v-model="value"
      class="input"
    />
    <div class="error-message">{{ errMsg }}</div>
  </div>
</template>

<script>
export default {
  name: 'InputTextValidation',
  props: {
    // 必須チェック
    required: {
      type: Boolean,
      default: false
    }

    // TODO チェックパターンとチェックに必要なパラメータを追加

  },
  data: () => {
    return {
      value: ''
    };
  },
  computed: {
    // エラー有無フラグ
    hasInputError() {
      return this.errMsg.length > 0;
    },
    // エラーメッセージ
    errMsg() {
      // TODO 複数パターンをチェック、エラー通知できるようにする

      if (this.required) {
        // 必須チェック
        if (!this.value.length) {
          return '入力してください。';
        }
      }

      // OK
      return '';
    }
  },
  method: {
    // TODO チェックパターンごとにメソッドを追加
  }
};
</script>

<style scoped>
.input-text-validation {
  text-align: left;
}

.input.error {
  border: 1px solid red;
}

.error-message {
  color: red;
}
</style>
