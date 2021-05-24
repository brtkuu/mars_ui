<template>
<section>
    <select @change="cameraChange($event)">
        <option value="FHAZ">FHAZ</option>
        <option value="MAST">MAST</option>
        <option value="RHAZ">RHAZ</option>
        <option value="" selected>All</option>
    </select>
    <select @change="roverChange($event)">
        <option value="Curiosity">Curiosity</option>
        <option value="Opportunity">Opportunity</option>
        <option value="Spirit">Spirit</option>
        <option value="" selected>All</option>
    </select>
    <div class="photos-container">
        <img class="photos-img" v-for="item in photos" :src="item.img_src" :key="item._id" />
    </div>
</section>
</template>
<script>
import axios from "axios";

export default {
    data() {
        return {
            photos: [],
            rover: "",
            camera: "",
        }
    },
    methods: {
        async roverChange(event) {
            this.rover = event.target.value;
            await this.getPhotos(this.camera, this.rover);
        },
        async cameraChange(event) {
            this.camera = event.target.value;
            await this.getPhotos(this.camera, this.rover);
        },
        async getPhotos(camera = "", rover = "") {
        const response = await axios.get(`http://localhost:8000/photos?${camera ? `camera_name=${camera}`:""}${rover ? `&rover_name=${rover}`:""}`, {headers: {"Access-Control-Allow-Origin": "*"}})
        this.photos = response.data;
    }
    },
    async mounted(){
        this.getPhotos();
    }
}
</script>
<style>
.photos-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    
}
.photos-img {
    justify-self: center;
    width: 300px;
    height: 250px;
}
</style>
