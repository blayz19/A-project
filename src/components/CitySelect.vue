<script setup>
    import IconLocation from '../icons/IconLocation.vue';
    import Button from './Button.vue';
    import Input from './Input.vue';
    import { onBeforeMount, onMounted, onUpdated, ref, watch } from 'vue';


    onMounted(()=>{
        console.log('City select m');
        
    });

    const emit = defineEmits({
        selectCity(payload){
            return payload ? true : false; 
        },
    });
    
    let isEdited = ref(false);
    let city = ref("Moscow");
    
    function select(){
        isEdited.value = false;
        emit('selectCity', city.value);
    }

    function edit(){
        isEdited.value = true;
    }

    watch(city, () =>{
        console.log(city.value);
    });
    
    onMounted(()=>{
        emit("selectCity", city.value);
    })

   

</script>

<template>
    <div class = "city-select">
        <div v-if="isEdited" class="city-input">
            <Input v-model="city" @keyup.enter="select()"/>
            <Button @click="select()">
            Сохранить
            </Button>
        </div>
        <Button v-else @click="edit()">
        <IconLocation></IconLocation>
        Изменить город
        </Button>
    </div>
</template>

<style scoped>
    .city-input{
        display: flex;
        gap: 12px;
    }
    .city-select{
        width: 420px;
    }
</style>