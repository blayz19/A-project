<script setup>
  import PanelRight from "./components/PanelRight.vue";
  import PanelLeft from "./components/PanelLeft.vue";
  import { onMounted, provide, ref, watch, computed } from "vue";
  import { cityProvide, dayProvide } from "./constants";
  import { API_ENDPOINT } from "./constants";
  

  let data = ref();
  let error = ref(null);
  let activeIndex = ref(0);
  let city = ref("Москва");
  provide(cityProvide, city);
  watch(city, ()=>{
    getCity(city.value);
  });
  onMounted(()=>{
    getCity(city.value);
  });
  
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
  const formattedDate = computed(() => {
  if (!data.value) return null;
  
  const dateStr = data.value.forecast.forecastday[activeIndex.value].date;
  return formatDate(dateStr);
});

// Функция форматирования даты
function formatDate(dateString) {
  const date = new Date(dateString);
  const options = { 
    day: 'numeric', 
    month: 'long', 
    year: 'numeric',
    weekday: 'long' 
  };
  
  // Русская локаль
  return date.toLocaleDateString('ru-RU', options);
}

// Если нужно раздельно день недели и дату
const weekDay = computed(() => {
  if (!data.value) return '';
  const dateStr = data.value.forecast.forecastday[activeIndex.value].date;
  const date = new Date(dateStr);
  return date.toLocaleDateString('ru-RU', { weekday: 'long' });
});

const dayMonthYear = computed(() => {
  if (!data.value) return '';
  const dateStr = data.value.forecast.forecastday[activeIndex.value].date;
  const date = new Date(dateStr);
  return date.toLocaleDateString('ru-RU', { 
    day: 'numeric', 
    month: 'long', 
    year: 'numeric' 
  });
});

</script>

<template>
  <main class="main">
    
    <div class="left-panel" v-if="data">
      <PanelLeft :day-data="{
          raw: data.forecast.forecastday[activeIndex].date,
          formattedDate: formattedDate,
          weekDay: weekDay,
          dayMonthYear: dayMonthYear
        }"
        :temp="data.forecast.forecastday[activeIndex].day.avgtemp_c" 
        :weather-code="data.forecast.forecastday[activeIndex].day.condition.code"
        :city="data.location.name"
        :country="data.location.country"
      >
      </PanelLeft>
    </div>
    
    <div class="right-panel">
      <PanelRight :data :error :active-index="activeIndex" 
      @select-index="(i)=> activeIndex = i">
      </PanelRight>
    </div>

  </main>
</template>

<style scoped>
  
  
  .main{
    display: flex;
    align-items: center;
    
  }
  .right-panel{
    background: var(--color-bg-main);
    padding: 60px 50px;
    border-radius: 0px 25px 25px 0px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
     margin-left: -30px; 
    z-index: 1;
    position: relative;
  }
  .left-panel{
    background: var(--gradient);
    min-width: 530px;
    min-height: 690px;
    border-radius: 30px;
    z-index: 99999; 
  }
  .day-cards{
    display: flex;
    width: 415px;
    gap: 1px;
    margin-top: 79px;
    margin-bottom: 71px;
  }
</style>
