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
        stat: data.current.humidity + "%",
        },
        {
        label: "Облачность",
        stat: data.current.cloud + "%",
        },
        {
        label: "Ветер",
        stat: data.current.wind_kph + "км/ч",
        },
    ]
    })
    

</script>

<template>
    <Error v-if="error" :error.value="errorDisplay"></Error>
      <div v-if="data">
        <Stat v-for="item in dataModified" v-bind="item" :key="item.label"></Stat>
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
      </div>
      <CitySelect ></CitySelect>
</template>

<style scoped>
    .day-cards{
    display: flex;
    width: 415px;
    gap: 1px;
    margin-top: 79px;
    margin-bottom: 71px;
  }
</style>