<script>
import { ref, onMounted } from 'vue'
import dayForecast from './dayForecast.vue'

export default {
  components: { dayForecast },
    props: ['dailyData'],
    setup(props) {
        const day_list = ref([])

        onMounted(async () => {
            let grouped_up = {}
            if (props.dailyData) {
                props.dailyData.forEach(async d => {
                    let date = d.date.split(' ')[0]
                    if (Object.keys(grouped_up).includes(date)) {
                        grouped_up[date].data.push(d)
                    } else {
                        grouped_up[date] = {data: [d], date}
                    }
                })
                console.log(grouped_up)
                let data_list = Object.values(grouped_up)
                data_list.forEach(async dl => {
                    let high
                    let low
                    let avg_list = []
                    let date = dl.date
                    dl.data.forEach((t, index) => {
                        if (index === 0) {
                            high = Math.round(t.temp_max)
                            low = Math.round(t.temp_min)
                        } else {
                            high = t.temp_max > high ? Math.round(t.temp_max) : high
                            low = t.temp_min < low ? Math.round(t.temp_min) : low
                        }
                        avg_list.push(t.temp)
                    })
                    let top_avg = 0
                    avg_list.forEach(a => top_avg = top_avg + a)
                    let avg = Math.round(top_avg/avg_list.length)
                    day_list.value.push({date, high, low, avg})
                })
            }
        })

        return {
            day_list
        }
    }
}
</script>

<template>
    <div>
        <h2>Daily Forecast</h2>
        <div v-for="d in day_list" :key="d.date">
            <day-forecast :data="d" />
        </div>
    </div> 
</template>

<style lang="sass">

</style>