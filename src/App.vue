<template>
  <div id="app">
    <block-modal v-if="globalState.modal.show"></block-modal>
    <router-view/>
  </div>
</template>

<script>
import BlockModal from './components/BlockModal.vue'
import { store } from './resources/store'

export default {
  name: 'App',
  data() {
    return {
      globalState: store.state
    }
  },
  components: {
    BlockModal
  }
}
</script>

<style>
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  body, html {
    background-color: #1a1a1a;
  }

  #app {
    padding: 30px;
    /* font-family: 'Avenir', Helvetica, Arial, sans-serif; */
    font-family: 'Roboto Mono', monospace;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #fff;
    background-color: #1a1a1a;
    /* height: 100vh; */
  }

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}

  @media only screen and (min-width: 1317px) {
    #app {
      width: 1128px;
      margin: auto;
    }
  }
</style>
