<template>
    <div>
        <ul v-if="blocksToDisplay.length > 0" class="blocks-list">
            <block-list-item v-for="block in blocksToDisplay" 
                :key="block.id"
                :block="block">
            </block-list-item>
        </ul>
        <div class="button-container">
            <button type="button" @click="getBlocks(lastBlockId)">Load more</button>
        </div>
    </div>
</template>

<script>
    import { JsonRpc } from 'eosjs';
    import BlockListItem from './BlockListItem.vue'

    const fetch = require('node-fetch');   
    const rpc = new JsonRpc('https://api.eosnewyork.io', { fetch }); 

    export default {
        data() {
            return {
                blocksToDisplay: []
            }
        },
        props: {
            headBlockNo: {
                required: true
            }
        },
        components: {
            BlockListItem
        },
        beforeMount() {
            this.getBlocks()
        },
        computed: {
            lastBlockId() {
                return this.blocksToDisplay[this.blocksToDisplay.length - 1].id
            }
        },
        methods: {
            getBlocks(blockNoOrId = this.headBlockNo) {
                let result = rpc.get_block(blockNoOrId)

                result.then((data) => {
                    console.log(data)
                    this.blocksToDisplay.push(data)
                    let previousBlockID = data.previous
                    console.log(previousBlockID)
                    // this.getBlock(previousBlockID)
                    for (let i = 0; i < 9; i++) {
                        let previousBlock = rpc.get_block(previousBlockID)
                        previousBlock.then((data) => {
                            this.blocksToDisplay.push(data)
                            previousBlockID = data.previous
                        })
                        .catch((error) => {
                            console.log(error)
                        })
                    }

                    console.log(this.blocksToDisplay)
                })
                .catch(error => {
                    console.log(error)
                })
            },
            async getBlock(blockID) {
                let result = await rpc.get_block(blockID)
                console.log(result)
            }
        }
    }
</script>

<style scoped>
    .blocks-list {
        margin-top: 20px;
        list-style: none;
        display: grid;
        grid-gap: 20px;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    }

    .button-container {
        margin-top: 20px;
    }

    .button-container button {
        color: #51c715;
        font-family: 'Roboto Mono', monospace;
        background-color: inherit;
        border-radius: 5px;
        border: 2px solid #51c715;
        padding: 7px 10px;
        font-weight: 700;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    .button-container button:hover {
        color: #fff;
        background-color: #51c715;
        transform: translateY(-3px)
    }
</style>