<template>
    <div class="flex px-3 py-6 rounded-md shadow-md bg-weather-secondary">
        <div class="flex flex-col flex-1 gap-2">
            <AnimatedPlaceHolder class="max-w-[50%]"/>
            <AnimatedPlaceHolder class="max-w-[40%]"/>
        </div>
        <div class="flex flex-col items-end flex-1 gap-2">
            <AnimatedPlaceHolder class="max-w-[50px] w-full"/>
            <AnimatedPlaceHolder class="max-w-[75px] w-full"/>
        </div>
    </div>
</template>

<script setup>
   import AnimatedPlaceHolder from '@/components/AnimatedPlaceHolder.vue';
</script>