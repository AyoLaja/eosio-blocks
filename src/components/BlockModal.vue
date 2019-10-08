<template>
    <div class="modal-container">
        <div class="modal" @click.stop>
            <div class="modal-header">
                <div class="modal-title">
                    <span>Block No: {{globalState.modal.details.block_num}}</span>
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

    export default {
        data() {
            return {
                globalState: store.state
            }
        },
        computed: {
            
        },
        methods: {
            closeModal() {
                this.globalState.modal.show = false
                const modalContainer = document.querySelector('.modal-container')
                modalContainer.removeEventListener('click', this.closeModal)
            }
        }
    }
</script>

<style lang="css" src="../styles/modalStyles.css" scoped></style>
<!--<style lang="scss" scoped>
     @import "../../styles/modalStyles.scss";
</style>-->
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