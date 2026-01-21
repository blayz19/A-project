<script setup>
    import { cityProvide } from '../constants';
    import IconLocation from '../icons/IconLocation.vue';
    import Button from './Button.vue';
    import Input from './Input.vue';
    import { inject, onBeforeMount, onMounted, onUpdated, ref, watch } from 'vue';

    const city = inject(cityProvide);
    const inputValue = ref(city.value);


    
    let isEdited = ref(false);
    
    
    function select(){
        isEdited.value = false;
        city.value = inputValue.value;
    }

    function edit(){
        isEdited.value = true;
    }

    
</script>

<template>
    <div class = "city-select">
        <div v-if="isEdited" class="city-input">
            <Input v-model="inputValue" @keyup.enter="select()" v-focus/>
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