<template>
    <div class="block-item" @click="openModal()">
        <div class="producer">
            <h2>{{block.producer}}</h2>
            <span>{{new Date(block.timestamp).toGMTString()}}</span>
        </div>
        <div class="block-details">
            <span>block no: {{block.block_num}}</span>
            <span>ref block prefix: {{block.ref_block_prefix}}</span>
        </div>
        <ul class="block-stats">
            <li>
                <span>{{block.transactions.length}}</span>
                <span>transactions</span>
            </li>
            <li>
                <span>{{block.schedule_version}}</span>
                <span>schedule version</span>
            </li>
        </ul>
        <hr/>
        <div class="button-container" @click.stop>
            <button type="button" @click="getRicardianContract">
                View ricardian actions
                <ion-icon name="ios-arrow-forward"></ion-icon>
            </button>
        </div>
    </div>
</template>

<script>
    import { JsonRpc } from 'eosjs';
    import { store } from '../resources/store'

    const fetch = require('node-fetch');   
    const rpc = new JsonRpc('https://api.eosnewyork.io', { fetch }); 
    
    export default {
        data() {
            return {
                globalState: store.state,
                ricardian: {}
            }
        },
        props: {
            block: {
                type: Object,
                required: true
            }
        },
        methods: {
            openModal(details = this.block, actionType = 'block') {
                const html = document.querySelector('html')
                html.style.overflowY = 'hidden'
                this.globalState.modal.show = true
                this.globalState.modal.actionType = actionType
                this.globalState.modal.details = details
            },
            async getRicardianContract() {
                this.ricardian = await rpc.get_abi('eosio')
                this.openModal(this.ricardian, 'abi')
            }
        }
    }
</script>

<style scoped>
    .block-item {
        position: relative;
        text-align: left;
        padding: 20px;
        box-shadow: 0 3px 6px 0 rgba(0,0,0,.1), 0 1px 3px 0 rgba(0,0,0,.08);
        background-color: #111;
        border-radius: 10px;
        cursor: pointer;
        overflow: hidden;
        transition: all 0.3s ease;
    }

    .block-item::before {
        position: absolute;
        counter-increment: a;
        content: counter(a);
        font-weight: 700;
        font-size: 250px;
        right: -40px;
        bottom: -40px;
        line-height: 1;
        color: #55555521;
    }
    
    .block-item:hover {
        transform: translateY(-3px);
    }
    
    .block-item div:not(:last-of-type),
    .block-item ul {
        margin-bottom: 10px;
    }
    
    .producer h2 {
        text-shadow: 1px 2px #4246ff;
    }
    
    .producer span {
        color: #4246ff;
        font-size: 10px;
        margin-top: 2px;
        display: block;
    }
    
    .block-details span {
        color: #ccc;
        font-size: 10px;
        display: block;
    }
    
    .block-stats {
        list-style: none;
        display: grid;
        grid-gap: 15px;
        grid-template-columns: auto auto;
    }
    
    .block-stats span {
        display: block;
    }
    
    .block-stats li span:first-of-type {
        font-size: 40px;
        color: #4246ff;
        font-weight: bold;
    }
    
    .block-stats li span:last-of-type {
        font-size: 12px;
    }
    
    hr {
        border-color: #333;
        margin-top: 20px;
        margin-bottom: 20px;
    }
    
    .button-container {
        text-align: center;
    }
    
    button {
        color: #4246ff;
        font-size: 12px;
        font-family: 'Roboto Mono', monospace;
        background-color: inherit;
        border-radius: 15px;
        border: 2px solid #4246ff;
        padding: 3px 10px 5px;
        font-weight: 500;
        cursor: pointer;
        transition: all 0.3s ease;
    }
    
    button:hover {
        color: #fff;
        background-color: #4246ff;
        transform: scale(1.02);
    }

    ion-icon {
        vertical-align: middle;
    }
</style>