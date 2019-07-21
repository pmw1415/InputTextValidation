<!--
TODO
・dateは書式で2パターンに分ける（スラッシュ区切りとハイフン区切り）
-->
<template>
  <div class="input-text-validation">
    <input
      :class="{ error: hasInputError }"
      :style="{ 'font-size': fontSize }"
      :placeholder="placeholder"
      :type="type"
      :value="value"
      class="input"
      @input="onInput"
    />
    <div
      v-for="(errMsg, index) in errMsgList"
      :key="index"
      class="validation-error"
    >
      <span
        :style="{ 'font-size': fontSize }"
        class="message"
      >
        {{ errMsg }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'InputTextValidation',
  props: {
    // input type
    type: {
      type: String,
      default: 'text'
    },
    // 入力初期値
    initValue: {
      type: String,
      default: ''
    },
    // フォントサイズ
    fontSize: {
      type: String,
      default: '14px'
    },
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
    },
    // 数値チェック
    numeric: {
      type: Boolean,
      default: false
    },
    // 半角英数チェック
    alphaNum: {
      type: Boolean,
      default: false
    },
    // 全角チェック
    zen: {
      type: Boolean,
      default: false
    },
    // 電話番号チェック
    tel: {
      type: Boolean,
      default: false
    },
    // メールアドレスチェック
    mail: {
      type: Boolean,
      default: false
    },
    // URLチェック
    url: {
      type: Boolean,
      default: false
    },
    // 日付チェック
    date: {
      type: Boolean,
      default: false
    }
  },
  data: () => {
    return {
      value: '',
      isInitDisp: true
    };
  },
  computed: {
    /**
     * プレースホルダー
     */
    placeholder() {
      let placeholder = '';
      if (this.tel) {
        placeholder = 'XXX-XXXX-XXXX';
      }
      if (this.mail) {
        placeholder = 'xxx@xx.xx';
      }
      if (this.url) {
        placeholder = 'http://〜, https://〜, ftp://〜';
      }
      if (this.date) {
        placeholder = 'YYYY/MM/DD, YYYY-MM-DD';
      }
      return placeholder;
    },

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
      // 初期表示時はエラーにしない
      if (this.isInitDisp) {
        return [];
      }

      const msgList = [];

      // 必須チェック
      if (this.isErrorRequired(this.value, this.required)) {
        msgList.push('入力してください。');
      }

      // 文字数チェック
      if (this.isErrorLengthMinMax(this.value, this.min, this.max)) {
        msgList.push(`${this.min}〜${this.max}文字を入力してください。`);
      } else if (this.isErrorLengthMin(this.value, this.min)) {
        msgList.push(`${this.min}文字以上を入力してください。`);
      } else if (this.isErrorLengthMax(this.value, this.max)) {
        msgList.push(`${this.max}文字以下を入力してください。`);
      }

      // 数値チェック
      if (this.isErrorNumeric(this.value, this.numeric)) {
        msgList.push(`半角数値で入力してください。`);
      }

      // 半角英数チェック
      if (this.isErrorAlphaNum(this.value, this.alphaNum)) {
        msgList.push(`半角英数で入力してください。`);
      }

      // 全角チェック
      if (this.isErrorZen(this.value, this.zen)) {
        msgList.push(`全角で入力してください。`);
      }

      // 電話番号チェック
      if (this.isErrorTel(this.value, this.tel)) {
        msgList.push(`電話番号を入力してください。`);
      }

      // メールアドレスチェック
      if (this.isErrorMailAddress(this.value, this.mail)) {
        msgList.push(`メールアドレスを入力してください。`);
      }

      // URLチェック
      if (this.isErrorUrl(this.value, this.url)) {
        msgList.push(`URLを入力してください。`);
      }

      // 年月日チェック
      if (this.isErrorDate(this.value, this.date)) {
        msgList.push(`日付(YYYY/MM/DD, YYYY-MM-DD)を入力してください。`);
      }

      return msgList;
    }
  },
  mounted() {
    if (this.initValue) {
      this.value = this.initValue;
    }
  },
  methods: {
    /**
     * 入力イベント
     * @param {Object} e イベントオブジェクト
     */
    onInput(e) {
      if (this.isInitDisp) {
        this.isInitDisp = false;
      }
      this.value = e.target.value;

      // 親に通知
      this.$emit('input', this.value);
    },

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
    isErrorLengthMax(value, max) {
      if (!max) {
        return false;
      }

      return value.length > max;
    },

    /**
     * 数値チェック
     *
     * numericが指定された場合にチェック実施。
     *
     * valueに半角数値以外が含まれる場合に入力エラー
     *
     * @param {String} value
     * @param {Boolean} numeric
     * @return {Boolean}
     */
    isErrorNumeric(value, numeric) {
      if (!numeric) {
        return false;
      }

      return !value.match(/^[0-9]+$/);
    },

    /**
     * 半角英数チェック
     *
     * alphaNumが指定された場合にチェック実施。
     *
     * valueに半角英数以外が含まれる場合に入力エラー
     *
     * @param {String} value
     * @param {Boolean} alphaNum
     * @return {Boolean}
     */
    isErrorAlphaNum(value, alphaNum) {
      if (!alphaNum) {
        return false;
      }

      return !value.match(/^[0-9A-Za-z]+$/);
    },

    /**
     * 全角チェック
     *
     * zenが指定された場合にチェック実施。
     *
     * valueに全角以外が含まれる場合に入力エラー
     *
     * @param {String} value
     * @param {Boolean} zen
     * @return {Boolean}
     */
    isErrorZen(value, zen) {
      if (!zen) {
        return false;
      }

      // 1バイト文字(¥x01-¥x7E)、半角カナ(¥xA1-¥xDF)が含まれないかチェック
      return !value.match(/^[^¥x01-¥x7E¥xA1-¥xDF]+$/);
    },

    /**
     * 電話番号チェック
     *
     * telが指定された場合にチェック実施。
     *
     * valueが以下パターン以外の場合に入力エラー
     * (1)1文字目がゼロ、2文字目がゼロ以外の数字
     * (2)数字が10〜11文字で、数字のみか電話番号の書式でハイフン区切りの数字
     *
     * @param {String} value
     * @param {Boolean} tel
     * @return {Boolean}
     */
    isErrorTel(value, tel) {
      if (!tel) {
        return false;
      }

      // 固定電話: XX-XXXX-XXXX, XXX-XXX-XXXX, XXXX-XX-XXXX, XXXXX-X-XXXX (10桁)
      // 携帯電話: 070-XXXX-XXXX, 080-XXXX-XXXX, 090-XXXX-XXXX (11桁)
      // IP電話: 050-XXXX-XXXX (11桁)
      // フリーダイヤル: 0120-XXX-XXX (10桁)

      if (!value.match(/^0[1-9]/)) {
        // 1文字目ゼロ、かつ2文字目ゼロ以外に該当しないためエラー
        return true;
      }

      if (value.match(/^\d{10}$|^\d{11}$/)) {
        // 数字のみ電話番号
        return false;
      }

      const regTel1 = /^\d{2}-\d{4}-\d{4}$|^\d{3}-\d{3}-\d{4}$|^\d{4}-\d{2}-\d{4}$|^\d{5}-\d-\d{4}$/;
      const regTel2 = /^\d{3}-\d{4}-\d{4}$/;
      const regTel3 = /^\d{4}-\d{3}-\d{3}$/;
      if (value.match(regTel1) || value.match(regTel2) || value.match(regTel3)) {
        // ハイフン付きの電話番号
        return false;
      }

      // 電話番号の書式じゃない
      return true;
    },

    /**
     * メールアドレスチェック
     *
     * mailが指定された場合にチェック実施。
     *
     * valueがx@x.xxの書式でない場合に入力エラー
     *
     * @param {String} value
     * @param {Boolean} mail
     * @return {Boolean}
     */
    isErrorMailAddress(value, mail) {
      if (!mail) {
        return false;
      }

      return !value.match(/\w{1,}[@][\w\-]{1,}([.]([\w\-]{1,})){1,3}$/);
    },

    /**
     * URLチェック
     *
     * urlが指定された場合にチェック実施。
     *
     * valueがhttp://～(https, ftp)の書式でない場合に入力エラー
     *
     * @param {String} value
     * @param {Boolean} url
     * @return {Boolean}
     */
    isErrorUrl(value, url) {
      if (!url) {
        return false;
      }

      return !value.match(/^(http|https|ftp)(:\/\/[-_.!~*\'()a-zA-Z0-9;\/?:\@&=+\$,%#]+)$/);
    },

    /**
     * 日付チェック
     *
     * dateが指定された場合にチェック実施。
     *
     * valueが日付書式(YYYY/MM/DD, YYYY-MM-DD)でない場合に入力エラー
     *
     * @param {String} value
     * @param {Boolean} date
     * @return {Boolean}
     */
    isErrorDate(value, date) {
      if (!date) {
        return false;
      }

      return !value.match(/^\d{4}\/\d{2}\/\d{2}$|^\d{4}-\d{2}-\d{2}$/);
    }
  }
};
</script>

<style scoped>
.input-text-validation {
  display: flex;
  flex-direction: column;
  text-align: left;
}

.input {
  border: solid 1px #b7b7b7;
  -webkit-border-radius: 4px;
  -moz-border-radius: 4px;
  border-radius: 4px;
  font-family: Arial, sans-serif;
  height: 1.6em;
  padding: 0 4px;
}

.input.error {
  border: 1px solid #c60019;
}

.input:focus {
  border: 1px solid #0074bf;
  outline: none;
}

.validation-error {
  margin-top: 4px;
}
.validation-error .message {
  background: #fff353;
  padding: 0 8px;
}
</style>
