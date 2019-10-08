<template>
  <div v-if="blockChain !== null" class="blockchain">
    <h1>{{ blockChain.account_name }} blockchain</h1>
    <div class="blocks">
      <block-list :headBlockNo="blockChain.head_block_num"></block-list>
    </div>
  </div>
</template>

<script>
import { JsonRpc } from 'eosjs';
import BlockList from './BlockList.vue'

const fetch = require('node-fetch');   
const rpc = new JsonRpc('https://api.eosnewyork.io', { fetch }); 

export default {
  data () {
    return {
      blockChain: null
    }
  },
  components: {
    BlockList
  },
  beforeMount() {
    this.getAccount()
  },
  methods: {
    async getAccount() {
      this.blockChain = await rpc.get_account("eosio") 
      console.log(this.blockChain)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 
</style>
