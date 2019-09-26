<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    
  </div>
</template>

<script>
import { Api, JsonRpc, RpcError } from 'eosjs';
import { JsSignatureProvider } from 'eosjs/dist/eosjs-jssig';  
const fetch = require('node-fetch');   

const defaultPrivateKey = "5JtUScZK2XEp3g9gh7F8bwtPTRAkASmNrrftmx4AxDKD5K4zDnr"; // bob
const signatureProvider = new JsSignatureProvider([defaultPrivateKey]);  

const rpc = new JsonRpc('https://api.eosnewyork.io', { fetch }); //http://127.0.0.1:8888
const api = new Api({ rpc, signatureProvider, textDecoder: new TextDecoder(), textEncoder: new TextEncoder() });

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted() {
    console.log(api)
    this.getInfo()
    this.getBlock()
  },
  methods: {
    async getInfo() {
      let result = await rpc.get_info()
      console.log(result)
    },
    async getBlock() {
      let result = await rpc.get_block(81424200)
      console.log(result)
    },
    getBlock2() {
      
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
