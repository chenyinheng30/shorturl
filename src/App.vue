<script setup lang="ts">
import { ref } from 'vue'
import { Vue3NextQrcode } from 'vue3-next-qrcode'
import CloudflareTurnstile from './components/CloudflareTurnstile.vue'

let LongUrl = ref("")
let ShortUrl = ref("")
let LongUrlInputMsg = ref("")
let ShortUrlInputMsg = ref("")
let QrcodeText = ref("https://tenet.chat")
let RetentionDays = ref(7)
let RetentionHours = ref(0)
let RetentionMinutes = ref(0)

function getUpperLetter(num: number) {
  return String.fromCharCode(64 + num);
}

function getLowerLetter(num: number) {
  return String.fromCharCode(96 + num);
}

function GetShortUrlPath(_Url: string) {
  var array = new Uint16Array(6);
  var path = "";
  window.crypto.getRandomValues(array);
  for (var i = 0; i < array.length; i++) {
    var tok = array[i] % 62;
    if (tok < 10) {
      path += tok;
    } else if (tok < 36) {
      path += getUpperLetter(tok - 9);
    } else {
      path += getLowerLetter(tok - 35);
    }
  }
  return path;
}

function GetLongUrlPath(_Url: string) {
  return "https://example.com"
}

function SetLongUrl(event: any) {
  LongUrlInputMsg.value = "";
  var InputValue = event.target.value;
  LongUrl.value = InputValue;
  if (InputValue != "") {
    if (InputValue.length < 1000) {
      var MatchUrl = /^(https|http):\/\/([A-z0-9]\.|[A-z0-9][A-z0-9-]*[A-z0-9]\.)+[A-z]{2,}(:[0-9]{1,5})?(\/.*)?$/;
      if (MatchUrl.test(InputValue)) {
        var Url = "https://tenet.chat/";
        Url += GetShortUrlPath(InputValue);
        ShortUrl.value = Url;
        QrcodeText.value = Url;
      } else {
        LongUrlInputMsg.value = "URL格式不匹配";
      }
    } else {
      LongUrlInputMsg.value = "URL长度需要小于1000";
    }
  }
}

function SetShortUrl(event: any) {
  ShortUrlInputMsg.value = "";
  let InputValue = event.target.value;
  ShortUrl.value = InputValue;
  if (InputValue != "") {
    if (InputValue.length <= 25) {
      var MatchUrl = /^https:\/\/tenet.chat\/[A-z0-9]{6}$/;
      if (MatchUrl.test(InputValue)) {
        var Url = GetLongUrlPath(InputValue);
        LongUrl.value = Url;
        QrcodeText.value = Url;
        return;
      }
    }
    ShortUrlInputMsg.value = "URL不正确";
  }
}
</script>

<template>
  <h1>本站建设中</h1>
  <p>
    本站将部署短链接服务，原站点迁移至
    <a href="https://blog.tenet.chat">blog.tenet.chat</a>
  </p>
  <p>
    <input class="mono_wieght" type="text" placeholder="输入长链接" :value="LongUrl" @change="SetLongUrl">
  <div class="error_msg">{{ LongUrlInputMsg }}</div>
  </p>
  <p>
    <input class="mono_wieght" type="text" placeholder="输入短链接" :value="ShortUrl" @change="SetShortUrl">
  <div class="error_msg">{{ ShortUrlInputMsg }}</div>
  </p>
  <p class="mono_wieght center_parent">
  <div>
    <section class="row_section">
      <section class="column_section box">
        <p class="rectangle">
        <section class="row_section little_rectangle">
          <div>天</div>
          <div>小时</div>
          <div>分钟</div>
        </section>
        <section class="row_section little_rectangle">
          <div>
            <input class="select_time" type="text" />
          </div>
          <div>
            <input class="select_time" type="text" />
          </div>
          <div>
            <input class="select_time" type="text" />
          </div>
        </section>
        </p>
        <p class="rectangle">
          <CloudflareTurnstile sitekey="0x4AAAAAAAeVj-x7WycONaEu" />
        </p>
      </section>
      <div class="box">
        <Vue3NextQrcode :text="QrcodeText" :size=200 />
      </div>
    </section>
  </div>
  </p>
</template>

<style scoped>
input[type=text] {
  height: 2rem;
  font-size: large;
  border: 2px solid #ccc;
  border-radius: 8px;
}

.mono_wieght {
  width: 50rem;
}

.row_section {
  display: flex;
  flex-direction: row;
}

.column_section {
  display: flex;
  flex-direction: column;
}

.center_parent {
  display: flex;
  align-items: center;
  justify-content: center;
}

.box {
  width: 15rem;
  height: 15rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.rectangle {
  width: 15rem;
  height: 15rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.error_msg {
  color: red;
  font-family: arial;
  font-size: small;
  text-align: left;
}

.select_time {
  width: 2rem;
  height: 1rem;
}

.little_rectangle {
  width: 15rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.little_rectangle>div {
  width: 33%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>: { target: { value: any; }; }: { target: { value: any; }; }
