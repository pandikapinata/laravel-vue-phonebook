<template>
<transition name="modal">
    <div class="modal modal-mask" style="display: block" >
        <div class="modal-dialog" role="document" >
            <div class="modal-content">
            <div class="modal-header">
                <slot name="header"><h5 class="modal-title">Modal title</h5></slot>
                <button type="button" class="close" aria-label="Close" @click="close()">
                <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <slot name="body">

                </slot>
            </div>
            <div class="modal-footer">
            <slot name="footer">

            </slot>
            </div>
            </div>
        </div>
    </div>
</transition>

</template>

<script>
    export default {
        methods:{
            close(){
                this.$emit('close')
            }
        }
    }
</script>
