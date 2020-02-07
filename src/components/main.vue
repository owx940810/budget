<template lang="pug">
main
    //- #budget
    //-     h5 current budget
    //-     h1 ${{ current.budget }}
    #remaining-budget(@click="toggleSelectedBudget")
        .day(:class="{active: budgetSelected === 'day'}")
            h3 ${{ remainingBudgetForTheDay }}#[span /{{ budgetForTheDay }}]
            p left today
        .month(:class="{active: budgetSelected === 'month'}")
            h3 ${{ budgetForTheMonth }}#[span /{{ current.budget }}]
            p left this month
    #statistics
        #week-stats
            .column(v-for="day in week")
                .bar-wrapper
                    .bar(:style="{height: ((day.spending / maxOfWeek * 100) || 0) + '%' }")
                .spending {{ day.spending }}
                .day {{ day.name }}

        .today
            p.day today
            p.spending ${{totalOfToday}}
        .yesterday
            p.day yesterday
            p.spendinig ${{ totalOfYesterday }}

</template>

<script>
import moment from "moment"
import _ from "lodash"
import Chart from 'chart.js'

export default {
  name: 'Main',
  props: {
    records: Array,
    history: Array,
    current: Object
  },

  data () {
    return {
        today: moment(),
        daysTillEndOfMonth: moment().endOf('month').diff(moment(), 'days'),
        hoursTillEndOfDay: moment().endOf('day').diff(moment(), 'hours'),
        budgetSelected: 'day'
    }
  },

  computed: {
    week () {
        let week = []
        for(let d = 0; d <=6; d++) {
            week.push({
                name: this.getDay(d),
                spending: 0
            })
        }

        this.records.forEach(record => {
            if(moment(record.timestamp).isSame(moment(), 'week')) {
                week[moment(record.timestamp).format('d')].spending += record.spending
            }
        })

        return week
    },

    maxOfWeek () {
        return _.maxBy(this.week, 'spending').spending
    },

    budgetForTheMonthExcludingToday () {
        let totalOfTheMonth = this.records.map(record => {
            console.log(record.timestamp)
            if (moment(record.timestamp).isSame(moment(), 'day')) {
                return 0
            }
            if (moment(record.timestamp).isSame(moment(), 'month')) {
                return record.spending
            }
            return 0
        })
        return this.current.budget - _.sum(totalOfTheMonth)
    },

    budgetForTheMonth () {
        let totalOfTheMonth = this.records.map(record => {
            if (moment(record.timestamp).isSame(moment(), 'month')) {
                return record.spending
            }
            return 0
        })

        return this.current.budget - _.sum(totalOfTheMonth)
    },

    budgetForTheDay () {
        let temp = (this.budgetForTheMonthExcludingToday / this.daysTillEndOfMonth)
        return Math.round(temp * 100) / 100
    },

    totalOfToday () {
        return _.sum(this.records.map(record => {
            if (moment(record.timestamp).isSame(moment(), 'day')) {
                return record.spending
            }
            return 0
        }))
    },

    totalOfYesterday () {
        return _.sum(this.records.map(recird => {
            if (moment(recird.timestamp).isSame(moment().subtract(1, 'days'), 'day')) {
                return recird.spending
            }
            return 0
        }))
    },

    remainingBudgetForTheDay () {
        let temp = this.budgetForTheDay - this.totalOfToday
        return Math.round(temp * 100) / 100
    }
  },

  mounted () {
    this.initChart()
  },

  methods: {
    initChart () {

    },

    getDay (d) {
        switch (d) {
            case 0:
                return 'M'
            case 1:
                return 'T'
            case 2:
                return 'W'
            case 3:
                return 'T'
            case 4:
                return 'F'
            case 5:
                return 'S'
            case 6:
                return 'S'
        }
    },

    toggleSelectedBudget () {
        if (this.budgetSelected === 'day') {
            this.budgetSelected = 'month'
        } else {
            this.budgetSelected = 'day'
        }
    }
  },
}
</script>

<style lang="sass" scoped>
    @import '../sass/mixins'

    main
        max-width: 700px
        width: 100%
        margin: 0 auto
        padding: 0 20px
        display: flex
        flex-flow: column nowrap
        justify-content: center
        overflow: hidden

    #budget
        display: flex
        flex-flow: column wrap
        text-align: center
        margin-bottom: 30px

        h5
            opacity: 0.6
            margin-bottom: 10px

    #remaining-budget
        position: relative
        display: flex
        flex-flow: column wrap
        text-align: center
        cursor: pointer
        margin-bottom: 30px

        .day
            opacity: 0
            span
                opacity: 0.3

        .month
            opacity: 0
            position: absolute
            top: 0
            left: 0
            width: 100%
            span
                opacity: 0.3

        .active
            transition: opacity 500ms
            opacity: 1

    #statistics
        display: grid
        grid-template-columns: 1fr 1fr
        grid-template-rows: 1fr auto
        grid-row-gap: 20px
        text-align: center
        background-color: white
        padding: 20px
        border-radius: 10px
        color: black

        #week-stats
            grid-column: 1 / span 2
            grid-row: 1
            display: grid
            grid-template-columns: repeat(7, 1fr)

            .column
                position: relative
                display: grid
                grid-template-rows: 120px 1fr 1fr
                grid-gap: 4px
                font-size: 14px

                .bar-wrapper
                    position: relative

                    .bar
                        position: absolute
                        bottom: 0
                        left: 50%
                        transform: translateX(-50%)
                        background-color: $green
                        width: 6px
                        border-radius: 3px
                        justify-self: center
                        transition: height 300ms ease-in-out

                .spending
                    opacity: 0.6
                .day
                    font-weight: bold

        .today
            grid-column: 1
            grid-row: 2

            p
                margin: 0
                &.day
                    margin-bottom: 6px

        .yesterday
            grid-column: 2
            grid-row: 2

            p
                margin: 0
                &.day
                    margin-bottom: 6px
</style>
