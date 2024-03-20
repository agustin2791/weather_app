<script>
import { ref, onMounted } from 'vue';
import HourView from './byHour.vue';

export default {
    components: { HourView },
    name: "Hourly",
    props: ['hourlyData'],
    setup(props) {
        const data = ref()

        onMounted(() =>{
            console.log(props.hourlyData)
            data.value = props.hourlyData.main
            
        })
        
        return {
            
        }
    },
}
</script>
<template>
    <div class="current-weather-container">
        <div class="current">
            <div class="humidity">
                {{ hourlyData.main.humidity }}%
            </div>
            <div class="temp">
                {{ Math.round(hourlyData.main.temp) }}
            </div>
            <div class="min">{{  Math.round(hourlyData.main.temp_min) }}<br>LOW</div>
            <div class="max">{{ Math.round(hourlyData.main.temp_max) }}<br>HIGH</div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.current-weather-container {
    width: 200px;
    margin: 10px auto;
    .current {
        display: flex;
        flex-wrap: wrap;
        background: #333;
        color: #fff;
        text-align: center;
        border-radius: 30px;
        .humidity {
            width: 100%;
            color: aqua;
            padding: 5px;
            font-size: 1.125rem;
            font-weight: 500;
        }
        .temp {
            width: 100%;
            padding: 20px;
            font-weight: bolder;
            font-size: 2rem;
            .small {
                font-size: 12px;
            }
        }
        .min, .max {
            width: 50%;
            padding: 5px;
        }
    }
}
</style>