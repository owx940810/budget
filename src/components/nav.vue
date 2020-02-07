<template lang="pug">
  nav
    button(@click="displayAddRecordOverlay")

</template>

<script>
export default {
  name: 'Nav',
  props: {
    // msg: String
  },

  methods: {
    displayAddRecordOverlay () {
        this.$emit('displayAddRecordOverlay')
    }
  }
}
</script>

<style lang="sass" scoped>
    button
        display: block
        margin: 0 auto 20px auto
        position: relative
        border-radius: 50%
        background-color: rgba(255, 255, 255, 0.3)
        border: 0
        color: white
        width: 50px
        height: 50px
        transition: transform 200ms
        outline: none

        &:hover, &:active, &:focus
            transform: scale(1.2)

        &::before
            content: ""
            position: absolute
            top: 50%
            left: 50%
            transform: translate(-50%, -50%)
            width: 6px
            border-radius: 5px
            height: 30px
            background-color: white

        &::after
            content: ""
            position: absolute
            top: 50%
            left: 50%
            transform: translate(-50%, -50%)
            width: 30px
            border-radius: 5px
            height: 6px
            background-color: white
</style>
