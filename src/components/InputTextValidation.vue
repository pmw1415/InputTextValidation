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
    /**
     * 入力エラー発生フラグ
     * 
     * @return {Boolean} 入力エラーありの場合true
     */
    hasInputError() {
      return this.errMsgList.length > 0;
    },

    /**
     * エラーメッセージリスト
     * 
     * 各入力チェックをして、検知したチェックパターンのエラーメッセージをarrayでセットする。
     * 未指定のチェックパターンはチェックしない。
     * 
     * @return {Array} エラーメッセージリスト。エラーなしの場合は空配列
     */
    errMsgList() {
      const msgList = [];

      // 必須チェック
      if (this.isErrorRequired(this.value, this.required)) {
        msgList.push('入力してください。');
      }

      // 文字数チェック
      if (this.isErrorLengthMinMax(this.value, this.min, this.max)) {
        msgList.push(`${this.min}〜${this.max}文字で指定してください。`);
      } else if (this.isErrorLengthMin(this.value, this.min)) {
        msgList.push(`${this.min}文字以上で指定してください。`);
      } else if (this.isErrorLengthMax(this.value, this.max)) {
        msgList.push(`${this.max}文字以下で指定してください。`);
      }

      return msgList;
    }
  },
  methods: {
    // TODO チェックパターンごとにメソッドを追加

    /**
     * 必須チェック
     * 
     * required===trueの場合にチェック実施。
     * 
     * valueが空の場合に入力エラー
     * 
     * @param {String} value
     * @param {Boolean} required
     * @return {Boolean}
     */
    isErrorRequired(value, required) {
      if (!required) {
        return false;
      }

      return !value.length;
    },

    /**
     * 文字数チェック: 最小、最大
     * 
     * min, max両方とも指定された場合にチェック実施。
     * 
     * value文字数がmin〜max範囲外の場合に入力エラー
     * 
     * @param {String} value
     * @param {Number} min
     * @param {Number} max
     * @return {Boolean}
     */
    isErrorLengthMinMax(value, min, max) {
      if (!min || !max) {
        return false;
      }

      return this.isErrorLengthMin(value, min) || this.isErrorLengthMax(value, max);
    },

    /**
     * 文字数チェック: 最小
     * 
     * minが指定された場合にチェック実施。
     * 
     * value文字数がmin未満の場合に入力エラー
     * 
     * @param {String} value
     * @param {Number} min
     * @return {Boolean}
     */
    isErrorLengthMin(value, min) {
      if (!min) {
        return false;
      }

      return value.length < min;
    },

    /**
     * 文字数チェック: 最大
     * 
     * maxが指定された場合にチェック実施。
     * 
     * value文字数がmaxを超えた場合に入力エラー
     * 
     * @param {String} value
     * @param {Number} max
     * @return {Boolean}
     */
    // 文字数チェック: 最大
    isErrorLengthMax(value, max) {
      if (!max) {
        return false;
      }

      return value.length > max;
    }
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
