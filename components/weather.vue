<template>
    <div>
        <section v-if="weather.season" class="weather">
            <h1>Weather</h1>
            <div><span class="weather-info">Date: {{weather.terrestrial_date}}</span><span class="weather-info">Season: {{weather.season}}</span></div>
            <div><span class="weather-info">Min temp.: {{weather.min_temp}} °C</span><span class="weather-info">Max temp.: {{weather.max_temp}} °C</span></div>
            <div><span class="weather-info">Sunrise: {{weather.sunrise}}</span><span class="weather-info">Sunset: {{weather.sunset}}</span></div>
        </section>
    </div>
</template>
<script>
import axios from "axios";

export default {
    data() {
        return {
            weather: {},
        }
    },
    async mounted() {
        const response = await axios.get("http://localhost:8000/weather", {headers: {"Access-Control-Allow-Origin": "*"}})
        console.log(response.data);
        this.weather = response.data;
    }
}
</script>
<style lang="css">
    .weather-info {
        padding: 5px;
    }
    .weather {
        border: 1px solid black;
        margin: 20px;
    }
</style>


