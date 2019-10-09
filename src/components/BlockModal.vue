<template>
    <div class="modal-container">
        <div class="modal" @click.stop>
            <div class="modal-header">
                <div class="modal-title">
                    <span>Block No: {{globalState.modal.details.block_num || globalState.modal.details.account_name}}</span>
                </div>
                <div class="close" title="close">
                    <ion-icon name="ios-close" @click="closeModal"></ion-icon>
                </div>
            </div>
            <div class="modal-body">
                <code v-html="globalState.modal.details">{{}}</code>
            </div>
        </div>
    </div>
</template>

<script>
    import { store } from '../resources/store'
    import mustache from 'mustache'

    export default {
        data() {
            return {
                globalState: store.state
            }
        },
        computed: {
            modalBodyContent() {
                return this.globalState.modal.details
            }
        },
        methods: {
            closeModal() {
                const html = document.querySelector('html')
                html.style.overflowY = 'scroll'
                this.globalState.modal.show = false
                this.globalState.modal.details = {}
            }
        }
    }
</script>

<style lang="css" src="../styles/modalStyles.css" scoped></style>
<style scoped>
    .modal {
        height: auto;
    }

    .modal-header {
        height: 10%;
        margin-bottom: 0px;
    }

    .modal-body {
        max-height: 500px;
        overflow-y: auto;
    }

    code {
        white-space: pre-wrap;    
        white-space: -moz-pre-wrap;  
        white-space: -pre-wrap;      
        white-space: -o-pre-wrap;   
        word-wrap: break-word; 
    }
</style>