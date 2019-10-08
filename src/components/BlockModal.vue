<template>
    <div class="modal-container">
        <div class="modal" @click.stop>
            <div class="modal-header">
                <div class="modal-title">
                    <span>Block</span>
                </div>
                <div class="close" title="close">
                    <ion-icon name="ios-close" @click="closeModal"></ion-icon>
                </div>
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
        height: 330px;
    }

    .modal-header {
        height: 10%;
        margin-bottom: 0px;
    }

    .modal-body {
        height: 65%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        z-index: 10;
    }
</style>