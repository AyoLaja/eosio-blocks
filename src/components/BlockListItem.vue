<template>
  <li class="block-item" @click="openModal">
      <div class="producer">
          <h2>{{block.producer}}</h2>
          <span>{{block.timestamp}}</span>
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
  </li>
</template>

<script>
    import { store } from '../resources/store'

    export default {
        data() {
            return {
                globalState: store.state
            }
        },
        props: {
            block: {
                type: Object,
                required: true
            }
        },
        methods: {
            openModal() {
                const html = document.querySelector('html')
                html.style.overflowY = 'hidden'
                this.globalState.modal.show = true
                this.globalState.modal.details = this.block
            }
        }
    }
</script>

<style scoped>
    .block-item {
        text-align: left;
        padding: 20px;
        box-shadow: 0 3px 6px 0 rgba(0,0,0,.1), 0 1px 3px 0 rgba(0,0,0,.08);
        background-color: #111;
        border-radius: 10px;
        cursor: pointer;
    }

    .block-item div,
    .block-item ul {
        margin-bottom: 10px;
    }

    .producer h2 {
        text-shadow: 1px 2px #51c715;
    }

    .producer span {
        color: #51c715;
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
        margin-top: 10px;
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
        font-weight: bold;
    }

    .block-stats li span:last-of-type {
        font-size: 12px;
    }
</style>