<template>
  <div class="input-text-validation">
    <input
      :class="{ error: hasInputError }"
      v-model="value"
      class="input"
    />
    <div
      v-for="(errMsg, index) in errMsgList"
      :key="index"
      class="error-message"
    >
      {{ errMsg }}
    </div>
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
    },
    // 最小文字数
    min: {
      type: Number,
      default: 0
    },
    // 最大文字数
    max: {
      type: Number,
      default: 0
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
      return this.errMsgList.length > 0;
    },

    // エラーメッセージリスト
    // 入力チェックして、エラーパターンのメッセージをセットする
    errMsgList() {
      const msgList = [];

      if (this.required) {
        // 必須チェック
        if (!this.value.length) {
          msgList.push('入力してください。');
        }
      }

      if (this.min && this.max) {
        // 文字数チェック: 最小、最大
        if (this.value.length < this.min || this.value.length > this.max) {
          msgList.push(`${this.min}〜${this.max}文字で指定してください。`);
        }
      } else if (this.min) {
        // 文字数チェック: 最小
        if (this.value.length < this.min) {
          msgList.push(`${this.min}文字以上で指定してください。`);
        }
      } else if (this.max) {
        // 文字数チェック: 最大
        if (this.value.length > this.max) {
          msgList.push(`${this.max}文字以下で指定してください。`);
        }
      }

      return msgList;
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
