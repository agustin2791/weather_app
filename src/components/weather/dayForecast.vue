<script>
export default {
    props: ['data'],
    setup() {
        
    },
}
</script>

<template>
    <div class="day-container">
        <div class="date">{{ data.date }}</div>
        <div class="avg">{{ data.avg }}</div>
        <div class="high-low">{{ data.low }}<br>{{ data.high }}</div>
    </div>
</template>

<style lang="scss" scoped>
.day-container {
    width: 95%;
    height: 55px;
    padding: 10px;
    margin: 10px;
    background: #aaa;
    color: #333;
    display: flex;
    align-content: middle;
    border-radius: 10px;
    .date, .avg, .high-low {
        text-align: center;
    }
    .date {
        width: 60%;
        font-size: 1.25rem;
    }
    .avg {
        width: 20%;
        font-size: 1.125rem;
        font-weight: bolder;
    }
    .high-low {
        width: 20%;
        align-self: center;
    }
}
</style>