<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
    
  </div>
</template>

<script>
import { JsonRpc } from 'eosjs';
const fetch = require('node-fetch');   
let options = {

}
const rpc = new JsonRpc('https://api.eosnewyork.io', fetch({ mode: 'no-cors'})); //http://127.0.0.1:8888 https://api.eosnewyork.io



export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted() {
    // console.log(fetch)
    // this.getInfo()
    // this.getBlock()
    var proxyUrl = 'https://cors-anywhere.herokuapp.com/';
    var url ='https://api.eosnewyork.io/v1/chain/get_block';
    var headers = {
      "Content-Type": "application/json"
    }
    var data = {
      "block_num_or_id": '1'
    }
    fetch(proxyUrl + url, { method: 'POST', headers: headers, body: data })
    .then(async (res) => {
      let data = await res.json()
      console.log(data)
      return data
    })
    .then((json) => {
      console.log(json);
    })
    .catch(error => console.log(error))
  },
  methods: {
    async getInfo() {
      let result = await rpc.get_info()
      console.log(result)
    },
    async getBlock() {
      console.log( await rpc.get_block(1))
      let result = await rpc.get_block(1)
      console.log(result)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
