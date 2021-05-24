<template>
    <section v-if="weather.season" class="weather">
        <div><span class="weather-info">Date: {{weather.terrestrial_date}}</span><span class="weather-info">Season: {{weather.season}}</span></div>
        <div><span class="weather-info">Min temp.: {{weather.min_temp}} °C</span><span class="weather-info">Max temp.: {{weather.max_temp}} °C</span></div>
    </section>
</template>
<script>
import axios from "axios";

export default {
    data() {
        return {
            weather: {
                terrestrial_date: undefined,
                sol: undefined,
                ls: undefined,
                season: undefined,
                min_temp: undefined,
                max_temp: undefined,
                sunrise: undefined,
                sunset: undefined,
            }
        }
    },
    async mounted() {
        const response = await axios.get("http://localhost:8000/weather", {headers: {"Access-Control-Allow-Origin": "*"}})
        console.log(response.data);
        this.weather = response.data;
        console.log(this.weather);
    }
}
</script>
<style lang="css">
    .weather-info {
        padding: 5px;
    }
</style>


