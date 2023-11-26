<template>
    <button>{{ figura }}</button>
</template>

<script setup lang="ts">
    defineProps({
        figura: String
    })    
</script>

<style scoped>
    button {
        width: 100px;
        height: 100px;
        font-size: 50px;
        background-color: transparent;
        border: 1px solid #42B883;
        margin: 5px;
    }
</style>