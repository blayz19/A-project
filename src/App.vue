<script setup>
  import Button from "./components/Button.vue";
  import Stat from "./components/Stat.vue";
  import IconComponent from "./icons/IconLocation.vue";
  import CitySelect from "./components/CitySelect.vue";
  import { nextTick, reactive, ref, computed } from "vue";
  import Error from "./components/Error.vue";
  import SunIcon from "./icons/weather/SunIcon.vue";
  import RainIcon from "./icons/weather/RainIcon.vue";
  import SunCloudIcon from "./icons/weather/SunCloudIcon.vue";
  import DayCard from "./components/DayCard.vue";


  const API_ENDPOINT = "https://api.weatherapi.com/v1";
  
  const errorMap = new Map([
    [1006,"Указанный город не найден"]
  ]);

  let data = ref();
  let error = ref(null);
  
  const errorDisplay = computed(() => {
    return errorMap.get(error.value?.error?.code);
  });
  
  const dataModified = computed(()=>{
    //if(!data.value){
    // return [];
    //}
    return [{
      label:"Влажность",
      stat: data.value.current.humidity + "%",
    },
    {
      label: "Облачность",
      stat: data.value.current.cloud + "%",
    },
    {
      label: "Ветер",
      stat: data.value.current.wind_kph + "км/ч",
    },
  ]
  })
  
  async function getCity(city){
    const params = new URLSearchParams({
      q: city,
      lang: "ru",
      key: "9c27f12ad6a1416ea45143603261901",
      days: "3",
    });
    const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`);
    if (res.status != 200){
      error.value = await res.json();
      data.value = null;
      return;
    }
    error.value = null;
    data.value = await res.json();
    
  }
  
</script>

<template>
  
  <main class="main">
    <Error :error.value="errorDisplay"></Error>
    <div v-if="data">
      <Stat v-for="item in dataModified" v-bind="item" :key="item.label"></Stat>
      <div class="day-cards" v-if="data">
        <DayCard v-for="item in data.forecast.forecastday" :weatherCode="item.day.condition.code" :temp="item.day.avgtemp_c" :date='new Date(item.date)' :key="item.date"></DayCard>
      </div>
    </div>

    <CitySelect @select-city = "getCity"></CitySelect>
  </main>
  
</template>

<style scoped>
  .main{
    background: var(--color-bg-main);
    padding: 60px 50px;
    border-radius: 25px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .day-cards{
    display: flex;
    width: 415px;
    gap: 1px;
    margin-top: 79px;
    margin-bottom: 71px;
  }
</style>
