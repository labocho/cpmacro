<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/> -->
    <div class="container">
      <div class="row justify-content-center">
        <div v-if="clipboardAvailable" class="col-sm-10">
          <h1 style="margin-bottom: 1em;">cpmacro</h1>
          <textarea v-model="input" class="form-control" rows="20" style="max-height: 25em; font-family: monospace;"/>
          <button @click="onClickStart" class="btn btn-lg btn-primary" style="width: 100%; margin: 2rem 0;" >開始</button>
          <table style="margin-bottom: 10em;">
            <tbody>
              <tr v-for="line in lines" :key="line.index" :class="{copied: line.copied}">
                <td style="padding: 5px 10px 5px 0;"><button @click="onClickCopy(line)" class="btn btn-outline-dark">Copy</button></td>
                <td style="text-align: left; font-family: monospace;">{{line.text}}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-else>
          <p>クリップボードが使えません。</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { ref } from "vue";
import HelloWorld from './components/HelloWorld.vue';

interface Line {
  text: string;
  copied: boolean;
}

export default Vue.extend({
  name: 'App',
  components: {
    HelloWorld
  },
  setup(_prop) {
    const clipboardAvailable = ref(navigator.clipboard !== undefined);
    const input = ref("");
    const lines = ref([] as Line[]);
    const onClickCopy = function(line: Line) {
      navigator.clipboard.writeText(line.text);
      line.copied = true;
    };
    const onClickStart = function() {
      lines.value = input.value.replace(/^\s+/, "").replace(/\s+$/, "").split("\n").map((line, index) => {
        return {index: index, text: line, copied: false};
      });
    };
    return {
      clipboardAvailable,
      input,
      lines,
      onClickCopy,
      onClickStart,
    }
  }
});
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

.copied {
  text-decoration: line-through;
}
</style>
