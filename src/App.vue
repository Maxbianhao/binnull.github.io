<template>
  <div id="app">
    <div class="tag">注：请先安装<a style="color: #62aec7" href="https://github.com/ChengOrangeJu/WebExtensionWallet" target="_blank">Chrome Nebulas-WebExtensionWallet</a>钱包插件</div>
    <button v-for="(item, index) in dapps" :key="index" @click="setFun(index)">{{index}}</button>
  </div>
</template>

<script>
import config from './config.json'
export default {
  name: 'app',
  data () {
    return {
      dapps: config
    }
  },
  methods: {
    setFun(i) {
      let dapp = this.dapps[i],
        agrs = dapp.agrs.replace('[','').replace(']','').split(','),
        callArgs = '['
      for(let i in agrs) {
        if(agrs[i][0] === '>') {
          callArgs = callArgs + '"' + agrs[i].replace(/\'/g,'').replace('>', '') + '",'
        } else if(agrs[i] != '') {
          if(agrs[i] === 'time') {
            callArgs = callArgs + '"' + new Date().getTime() + '",'
          }
        } else {
          callArgs = callArgs + '"' + Number(Math.random().toString().substr(3,0) + Date.now()).toString(36) + i + '",'
        }
      }
      callArgs = callArgs.substring(0, callArgs.length - 1) + ']'
      saveFun(callArgs, dapp);
    }
  }
}

function saveFun(callArgs, dapp) {
  let to = dapp.address,
    value = "0",
    callFunction = dapp.function

  nebPay.call(to, value, callFunction, callArgs, {});
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

button {
  height: 30px;
  width: 200px;
  font-size: 14px;
  cursor: pointer;
  margin: 0 auto;
  margin-top: 20px;
  display: block;
}
</style>
