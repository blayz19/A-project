<script setup>
    import Error from './Error.vue';
    import Stat from './Stat.vue';
    import DayCard from './DayCard.vue';
    import CitySelect from './CitySelect.vue';
    import { ref, computed } from 'vue';
    
    const{error,data, activeIndex} = defineProps({
        error: Object,
        data: Object,
        activeIndex: Number,
    });

    const emit = defineEmits(['select-index', 'select-city']);

    const errorMap = new Map([
        [1006,"Указанный город не найден"]
    ]);

    
    
    const errorDisplay = computed(() => {
        return errorMap.get(error?.error?.code);
    });
    
    const dataModified = computed(()=>{
        if(!data){
         return [];
        }
        return [{
        label:"Влажность",
        stat: data.forecast.forecastday[activeIndex].day.avghumidity + "%",
        },
        {
        label: "Вероятность осадков",
        stat: data.forecast.forecastday[activeIndex].day.daily_chance_of_snow + "%",
        },
        {
        label: "Ветер",
        stat: data.forecast.forecastday[activeIndex].day.maxwind_kph + "км/ч",
        },
    ]
    })
    

</script>

<template>
    <Error v-if="error" :error.value="errorDisplay"></Error>
      <div v-if="data" class="data">
        <Stat class="statistic" v-for="item in dataModified" v-bind="item" :key="item.label"></Stat>
        <div class="container">
            <div class="day-cards" v-if="data">
            <DayCard v-for="(item,i) in data.forecast.forecastday" 
                :weatherCode="item.day.condition.code" 
                :temp="item.day.avgtemp_c" 
                :date='new Date(item.date)' 
                :is-active="activeIndex==i" 
                :key="item.date"
                @click="()=>{emit('select-index',i)}">
            </DayCard>
            </div>
        
        <CitySelect ></CitySelect>
        </div>
      </div>
</template>

<style scoped>
    .data{
        display: flex;
        flex-direction: column;
    }
    .day-cards{
        display: flex;
        width: 415px;
        gap: 1px;
        margin-top: 79px;
        margin-bottom: 71px;
    }
    .container{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

    }
</style>