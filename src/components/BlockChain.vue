<template>
  <div v-if="!error" class="block-chain">
    <div v-if="blockChain !== null" class="blocks">
      <h1>{{ blockChain.account_name }} blockchain</h1>
      <p>Latest blocks on the eosio blockchain</p>
      <block-list :headBlockNo="blockChain.head_block_num"></block-list>
    </div>
  </div>
  <error v-else></error>
</template>

<script>
  import { JsonRpc } from 'eosjs';
  import BlockList from './BlockList.vue'
  import Error from './Error.vue'
  
  const fetch = require('node-fetch');   
  const rpc = new JsonRpc('https://api.eosnewyork.io', { fetch }); 
  
  export default {
    data () {
      return {
        blockChain: null,
        error: false
      }
    },
    components: {
      BlockList,
      Error
    },
    beforeMount() {
      this.getAccount()
    },
    methods: {
      async getAccount() {
        let account = rpc.get_account("eosio")

        account.then((data) => {
          this.blockChain = data
        })
        .catch((error) => {
          console.log(error)
          this.error = true
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1 {
    font-family: 'Lalezar', cursive;
    /* color: #4246ff; */
    text-transform: uppercase;
  }
</style>
