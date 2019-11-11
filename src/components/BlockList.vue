<template>
    <div>
        <ol v-if="blocksToDisplay.length > 0" class="blocks-list">
            <transition-group tag="li" class="transition-group" name="slide-fade">
                <block-list-item v-for="block in blocksToDisplay" 
                    :key="block.id"
                    :block="block">
                </block-list-item>
            </transition-group>
        </ol>
        <div class="button-container">
            <button type="button" @click="getMore">Load more</button>
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
                blocksToDisplay: [],
                count: 1
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
                return this.blocksToDisplay[this.blocksToDisplay.length - 1].previous
            }
        },
        methods: {
            async getBlocks(blockNoOrId = this.headBlockNo) {
                if (this.count % 10 !== 0) {
                    let result = await rpc.get_block(blockNoOrId)
                    this.blocksToDisplay.push(result)
                    let previousID = result.previous
                    this.getBlocks(previousID)
                    this.count++
                }
            },
            async getMore() {
                this.getBlocks(this.lastBlockId)
            }
        }
    }
</script>

<style scoped>
    .blocks-list {
        margin-top: 20px;
        list-style: none;
        counter-reset: a;
        
    }

    .transition-group {
        display: grid;
        grid-gap: 20px;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    }

    .button-container {
        margin-top: 20px;
    }

    .button-container button {
        color: #fff;
        font-size: 15px;
        font-family: 'Roboto Mono', monospace;
        background-color: inherit;
        border-radius: 5px;
        border: 2px solid #4246ff;
        padding: 7px 10px;
        font-weight: 700;
        text-shadow: 1px 2px #4246ff;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    .button-container button:hover {
        color: #fff;
        background-color: #4246ff;
        transform: translateY(-3px)
    }
</style>