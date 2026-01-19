<script setup>
    const data =  defineModel({type: String, required: true,});
    
</script>

<template>
    <input v-model="data" class="input" placeholder="Введите город"/>
</template>

<style scoped>
    .input{
        color:var(--color-primary);
        border-radius: 10px;
        width: 260px;
        border: none;
        font-size: 18px;
        height: 53px;
        background: var(--color-bg-card);
        box-shadow: 1px 2px 4px 0px var(--color-bg-main);
        padding: 15px 20px;
    }
    .input::placeholder{
        color:#3f4958;
    }
</style>