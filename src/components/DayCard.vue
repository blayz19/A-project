<script setup>
import RainIcon from '../icons/weather/RainIcon.vue';
import SunCloudIcon from '../icons/weather/SunCloudIcon.vue';
import SunIcon from '../icons/weather/SunIcon.vue';

    const {date, temp, weatherCode} = defineProps({
        date: Date,
        temp: Number,
        weatherCode: Number,
    });

</script>

<template>
    
    <button class="card">
        <SunIcon v-if="weatherCode <= 1003" />
        <SunCloudIcon v-else-if="weatherCode >=1003 && weatherCode <=1009" />
        <RainIcon v-else-if="weatherCode > 1010 " />
        <p class="day-card_day">{{ date.toLocaleDateString('ru-RU', {weekday:"short"}) }}</p>
        <p class="day-card_temp">{{ temp }} °C</p>
    </button>
</template>

<style scoped>
    .card{
        
        width: 100%;
        border: none;
        padding: 15px 24px;
        background-color: var(--color-bg-card);
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        border-radius: 10px;
        box-shadow: 1px 2px 4px 0px #222831;
        gap:15px;
        color: var(--color-primary);
        cursor: pointer;
    }
    .day-card_day{
        font-size: 20px;
        font-weight: 400;
    }
    .day-card_temp{
        font-size: 20px;
        font-weight: 700;
    }
    .card:hover{
        background-color:#3a434f ;
    }
</style>