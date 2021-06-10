<template>
<div>
    <section v-if="apodStatus" class="apod">
        <h1>Astronomy picture of the day</h1>
        <img v-if="apod.media === 'image'" class="apod-pic" :src="apod.url" />
         <iframe v-if="apod.media === 'video'" width="420" height="315"
            :src="apod.url">
        </iframe> 
        <p>{{apod.title}}</p>
    </section>
    <loading v-if="!apodStatus" />
</div>
</template>
<script>
import axios from "axios";
import loading from './loading.vue';

export default {
    components: { loading },
    data() {
        return {
            apod: {},
            apodStatus: false,
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
        await this.getApodData();
        this.apodStatus = true;
    }
}
</script>
<style>
.apod {
    margin: 20px;
}
.apod-pic {
    width: 300px;
    height: 250px;
}
</style>


