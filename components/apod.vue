<template>
    <section v-if="apod.date" class="apod">
        <h1>Astronomy picture of the day</h1>
        <img class="apod-pic" :src="apod.hdurl" />
    </section>
</template>
<script>
import axios from "axios";

export default {
    data() {
        return {
            apod: {}
        }
    },
    methods: {
        async getApodData(date) {
        date = date? date :`${new Date().getFullYear()}-${new Date().getMonth() + 1 < 9 ?  `0${new Date().getMonth() + 1}` : new Date().getMonth() + 1}-${new Date().getDate()}`;
        const response = await axios.get(`http://localhost:8000/apod?date=${date}`, {headers: {"Access-Control-Allow-Origin": "*"}})
        this.apod = response.data[0];
        }
    },
    async mounted() {
        this.getApodData();
    }
}
</script>
<style>
.apod-pic {
    width: 300px;
    height: 250px;
}
</style>


