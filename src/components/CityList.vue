<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)"/>
    </div>
    <p v-if="savedCities.length == 0">
    No locations Saved</p>
</template>

<script setup>
    import { ref } from "vue";
    import axios from "axios";
    import CityCard from "./CityCard.vue";
    import { useRouter } from "vue-router";

    const savedCities = ref([]);
    const getCities = async () => {
        if(localStorage.getItem('savedCities')){
            savedCities.value = JSON.parse(localStorage.getItem('savedCities'));
        }

        const request = [];
        savedCities.value.forEach((city) => {
            request.push(
                axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lon}&appid=${import.meta.env.VITE_OPENW_API}&units=imperial`));
        });

        const weatherData = await Promise.all(request);

        weatherData.forEach((value, index) => {
            savedCities.value[index].weather = value.data;
        });

    }

    await getCities();

    const router = useRouter();
    const goToCityView = (city) => {
        router.push({
            name : 'city',
            params: {state: city.state, city: city.city},
            query: {lat: city.coords.lat, lon: city.coords.lon, id: city.id}
        })
    }

    await new Promise(resolve => setTimeout(resolve, 1000));
</script>
