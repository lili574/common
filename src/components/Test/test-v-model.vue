<template>
    <div class="test">
        <input type="text" :value="lili" @paste="handlePaste">
    </div>
</template>

<script>
    export default {
        name: "TestVModel",
        model: {
            prop: 'lili',
            event: 'paste'
        },
        props: {
            lili: {
                type: String
            }
        },
        methods:{
            handlePaste(e){
                console.log(e.target.value);
                this.$emit('paste', e.target.value)
            }
        }
    }
</script>

<style scoped>

</style>
