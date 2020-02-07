<template lang="pug">
    #overlay
        input(ref="input", type="tel", v-model="spending")
        button(@click="addNewRecord") confirm

</template>

<script>
export default {
    name: 'Overlay',
    data () {
        return {
            spending: ''
        }
    },

    methods: {
        addNewRecord () {
            this.$emit('addNewRecord', Number(this.spending))
        }
    },

    mounted () {
        this.$refs.input.focus()
    }
}
</script>

<style lang="sass" scoped>
#overlay
    position: fixed
    top: 0
    left: 0
    height: 100%
    width: 100%
    display: grid
    place-items: center center
    background-color: rgba(0, 0, 0, 0.8)

    input
        display: block
        font-size: 64px
        background-color: transparent
        outline: none
        border: 0
        color: white
        text-align: center
        width: calc(100% - 40px)

    button
        position: absolute
        bottom: 20px
        left: 50%
        transform: translateX(-50%)
        color: white
        padding: 10px
        border-radius: 10px
        background-color: grey
        border: 0
        font-size: 24px
        outline: 0

</style>
