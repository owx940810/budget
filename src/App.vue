<template lang="pug">
    #app
        header-component
        main-component(:records="records", :history="history", :current="current")
        nav-component(@displayAddRecordOverlay="displayAddRecordOverlay")
        overlay(v-if="overlayState", @addNewRecord="addNewRecord")
</template>

<script>
import header from './components/header.vue'
import main from './components/main.vue'
import nav from './components/nav.vue'
import overlay from './components/overlay.vue'

export default {
    name: 'app',
    components: {
        "header-component": header,
        "main-component": main,
        "nav-component": nav,
        "overlay": overlay
    },
    data () {
        return {
            records: [],
            history: [],
            current: {
                budget: 220
            },

            overlayState: false
        }
    },
    methods: {
        displayAddRecordOverlay () {
            this.overlayState = true
        },

        addNewRecord (spending) {
            this.overlayState = false
            if(!Number(spending)) {
                return
            }

            this.records.push({
                spending: spending,
                timestamp: Date.now()
            })
        }
    }
}
</script>

<style lang="sass">
#app
    height: 100%
    display: grid
    grid-template-rows: auto 1fr auto
    grid-template-columns: 1fr

</style>
