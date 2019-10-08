<template>
  <div>
      <p>previous: {{ blockInfo.previous}}</p>
  </div>
</template>

<script>
    import { JsonRpc } from 'eosjs';
    import { async } from 'q';
    const fetch = require('node-fetch');   
    const rpc = new JsonRpc('https://api.eosnewyork.io', { fetch }); 

    export default {
        data() {
            return {
                blockInfo: {},
                blocksToDisplay: []
            }
        },
        props: {
            headBlockNo: {
                required: true
            }
        },
        beforeMount() {
            this.getBlocks()
        },
        methods: {
            getBlocks(blockNoOrId = this.headBlockNo) {
                let result = rpc.get_block(blockNoOrId)

                result.then((data) => {
                    console.log(data)
                    this.blocksToDisplay.push(data)
                    let previousBlockID = data.id || data.block_num
                    console.log(previousBlockID)
                    
                    for (let i = 0; i < 9; i++) {
                        this.getBlock(previousBlockID)
                        // let previousBlock = rpc.get_block(previousBlockID)
                        // previousBlock.then((data) => {
                        //     this.blocksToDisplay.push(data)
                        //     previousBlockID = data.id || data.block_num
                        // })
                        // .catch((error) => {
                        //     console.log(error)
                        // })
                    }

                    console.log(this.blocksToDisplay)
                })
                .then((previousBlockID) => {
                    
                })
                .catch(error => {
                    console.log(error)
                })
            },
            async getBlock(blockID) {
                console.log(await rpc.get_block(blockID))
            }
        }
    }
</script>

<style>

</style>