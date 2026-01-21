<script setup>
    import IconLocation from '../icons/IconLocation.vue';
import RainIcon from '../icons/weather/RainIcon.vue';
    import SunCloudIcon from '../icons/weather/SunCloudIcon.vue';
    import SunIcon from '../icons/weather/SunIcon.vue';

    const {dayData,temp, weatherCode} = defineProps({
        dayData: Object,
        temp: Number,
        weatherCode: Number,
        city: String,
        country: String,
    });
    function whatWeather(weatherCode){
        if(weatherCode >= 1063){
            return "Дождливо/Снежно"
        }
        else if(weatherCode >1006 && weatherCode < 1063){
            return "Облачно"
        }
        else{
            return "Солнечно"
        }
    }
</script>

<template>
    <div class="panel">
        <div class="day-geo">
            <template v-if="dayData">
                <h2 class="week-day">{{ dayData.weekDay.toUpperCase() }}</h2>
                <p class="date">{{ dayData.dayMonthYear.replace(' г.', '') }}</p>
                <p class="location">{{ city}}, {{ country }} <IconLocation></IconLocation></p>
            </template>
        </div>
        <div class="about-weather">
            <div class="icons">
                <RainIcon v-if="weatherCode >= 1063" :size="95"></RainIcon>
                <SunCloudIcon v-if="weatherCode >1006 && weatherCode < 1063" :size="95"></SunCloudIcon>
                <SunIcon v-if="weatherCode <= 1003" :size="95"></SunIcon>
            </div>
            <div class="info">
                <p class="temp">{{ temp }} °C</p>
                <p class="weather">{{ whatWeather(weatherCode) }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .panel{
        min-height: inherit;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }
    .week-day{
        font-size: 37px;
        font-weight: 700;
    }
    .date{
        font-size: 22px;
    }
    .about-weather{
        margin-bottom: 20px;
        display: flex;
        flex-direction: column;
    }
    .temp{
        font-size:50px;
        font-weight: 700;
        margin-left: 32px;
        margin-top: 30px;
        margin-bottom: 20px;

    }
    .icons{
        margin-left: 64px;
    }
    .day-geo{
        margin-left: 32px;
    }
    .weather{
        margin-left: 32px;
        font-size: 30px;
        font-weight: 700;
        margin-top: 0px;
        margin-bottom: 40px;
    }
    .location{
        font-size: 20px;
        font-weight: 400px;
    }
</style>