<template>
  <div id="app">
    <div>
      <h3>Input Event Demo</h3>
      <input @input="onInput" @keydown="onKeyDown" />
      <p>inputType: {{ inputType }}</p>
      <p>PC Track: {{ pcTrack }}</p>
      <button @click="reset">reset</button>
      <p>IOS Track: {{ iosTrack }}</p>
      <p>Android Track: {{ androidTrack }}</p>
    </div>
  </div>
</template>

<script>
const ANDROID = "Android";
const IOS = "IOS";
const NULL = "UNKNOWN OS";

const INPUT_TYPES = {
  UNCONVERTED_STATE: "insertCompositionText",
  CONVERETED_STATE: "insertFromComposition",
  DELETE_UNCONVERTED: "deleteCompositionText",
  DELETE_CONVERTED: "deleteContentBackward"
};

const KEYDOWN_MAP = {
  BACKSPACE: "Backspace"
}

const ua = window.navigator.userAgent.toLowerCase();

const isMobile = ua.indexOf('mobile') > 0;
const osName = (function(){
  if (isMobile) {
    return (ua.indexOf('iphone') != -1 || ua.indexOf('ipad') != -1 ) ? IOS : ANDROID;
  }
  return NULL;
}())


export default {
  name: "App",
  data: function() {
    return {
      datas: [],
      pcTrack: [],
      inputType: "",
      androidTrack: [],
      iosTrack: [],
    };
  },
  methods: {
    onInput: function(e) {
      this.inputType = e.inputType;
      if (osName ===  IOS) {
        this.iosInput(e);
      }
    },
    onKeyDown: function(e) {
      switch (osName) {
        case ANDROID: {
          this.androidInput(e);
          break;
        }
        case NULL: {
          this.pcInput(e);
          break;
        }
      }
    },
    iosInput: function(e) {
      console.log(e);
      switch (e.inputType) {
        case INPUT_TYPES.UNCONVERTED_STATE: {
          this.iosTrack.push(e.data);
          break;
        }
        case INPUT_TYPES.CONVERETED_STATE: {
          this.iosTrack.push(e.data);
          break;
        }
        case INPUT_TYPES.DELETE_UNCONVERTED: {
          this.iosTrack.pop();
          break;
        }
        case INPUT_TYPES.DELETE_CONVERTED: {
          this.iosTrack.pop();
          break;
        }
        default:
      }
      if (e.target.value === "") {
        this.iosTrack = [];
      }
    },
    androidInput: function(e) {
      switch (e.key) {
        case KEYDOWN_MAP.BACKSPACE: {
          this.androidTrack.pop();
          break;
        }
        default:
          this.androidTrack.push(e.key);
      }
    },
    pcInput: function(e) {
      this.pcTrack.push(e.key);
    },
    reset: function() {
      this.data = "";
      this.iosTrack = [];
      this.pcTrack = [];
      this.androidTrack = [];
    }
  }
};
</script>

<style scoped>
input {
  font-size: 16px;
  border: 1px solid black;
}
button {
  height: 20px;
  width: 40px;
  border: 1px solid black;
}
</style>