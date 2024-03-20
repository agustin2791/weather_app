<script>
import { ref, onMounted } from 'vue'
import hourly from './hourly.vue';
import axios from 'axios';
import daily from './daily.vue';

const API_KEY = '482944e26d320a80bd5e4f23b3de7d1f'
export default {
    components: { hourly, daily },
    name: 'WeatherHome',
    setup() {
        // DATA
        const search_value = ref('')
        const focus = ref('')
        const default_tabs = ref(['Rio de Janeiro', 'Beijing', 'Los Angeles'])
        const hourly_data = ref()
        const current_data = ref()
        const updates = ref(0)
        // FUNCTIONS
        const changeLocation = async (location) => {
            focus.value = location
            await getCurrentData()
            await getHourlyWeatherData()
        }

        const searchByCity = async () => {
            await changeLocation(search_value.value)
        }
        const getCurrentData = async () => {
            let api_url = `https://api.openweathermap.org/data/2.5/weather?q=${focus.value}&appid=${API_KEY}&units=imperial`
            let _data = await axios({
                method: 'get',
                url: api_url
            })

            current_data.value = _data.data
            focus.value = _data.data.name
            updates.value = updates.value + 1
        }

        const getHourlyWeatherData = async () => {
            let api_url = `https://api.openweathermap.org/data/2.5/forecast?q=${focus.value}&appid=${API_KEY}&units=imperial`;
            let _data = await axios({
                method: 'get',
                url: api_url
            })
            if (_data.data.list){
                hourly_data.value = _data.data.list.map(d => {
                    return {...d.main,
                        datetime: new Date(d.dt),
                        date: d.dt_txt,
                        weather: d.weather
                    }
                }).sort((a, b) => {
                    if (a.datetime > b.datetime) return 1
                    if (a.datetime > b.datetime) return -1
                    return 0
                })
            }

            updates.value = updates.value + 1
        }
        onMounted(async () => {
            focus.value = default_tabs.value[0]
            await getHourlyWeatherData()
            await getCurrentData()
        })
        return {
            focus,
            default_tabs,
            hourly_data,
            current_data,
            updates,
            search_value,
            changeLocation,
            searchByCity
        }
    },
}
</script>

<template>
    <div class="weather-container">
        <h2>Weather Forecast</h2>
        <div class="search-container">
            <input v-model="search_value" class="search-bar" />
            <button @click="searchByCity">Search</button>
        </div>
        <div class="nav-tabs-container">
            <div v-for="t in default_tabs" :key="t" class="nav-tab" @click="changeLocation(t)" :class="[t === focus ? 'active' : '']">
                {{ t }}
            </div>
        </div>
        <h2 style="text-align: center;">{{ focus }}</h2>
        <div class="hourly-container" v-if="current_data">
            <hourly :hourlyData="current_data" :key="updates" />
        </div>
        <div class="forcast-container" v-if="hourly_data">
            <daily :key="updates" :dailyData="hourly_data" />
        </div>
    </div>
</template>

<style lang="scss" scoped>
.weather-container {
    width: 400px;
    padding: 20px 20px 40px 20px;
    background: rgb(200, 225, 233);
    color: #333;
    border-radius: 10px;
}
.nav-tabs-container {
    display: flex;
    width: 100%;
    margin: 10px 0;
    .nav-tab {
        width: calc(100% / 3);
        padding: 5px;
        text-align: center;
        cursor: pointer;
        &.active {
            border-bottom: solid thick rgb(15, 15, 200);
        }
    }

}
</style>