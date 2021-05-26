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
    <loading v-if="!photosStatus" />
    <div v-if="photosStatus" class="photos-container">
        <img class="photos-img" v-for="i in iterations" :src="photos[i-1].img_src" :key="i" :id="i-1" />
    </div>
    <button @click="morePhotos" v-if="iterations > 8">More</button>
</section>
</template>
<script>
import axios from "axios";
import loading from './loading.vue';

export default {
    components: { loading },
    data() {
        return {
            photos: [],
            rover: "",
            camera: "",
            photosStatus: false,
            iterations: 0
        }
    },
    methods: {
        morePhotos() {
            if(this.iterations + 9 > this.photos.length){
                this.iterations = this.photos.length;
            } else {
                this.iterations = this.iterations + 9;
            }

        },
        async roverChange(event) {
            this.rover = event.target.value;
            this.photosStatus = false;
            await this.getPhotos(this.camera, this.rover);
        },
        async cameraChange(event) {
            this.camera = event.target.value;
            this.photosStatus = false;
            await this.getPhotos(this.camera, this.rover);
        },
        async getPhotos(camera = "", rover = "") {
            console.log(this.photosStatus);
            const response = await axios.get(`http://localhost:8000/photos?${camera ? `camera_name=${camera}`:""}${rover ? `&rover_name=${rover}`:""}`, {headers: {"Access-Control-Allow-Origin": "*"}})
            this.photos = response.data;
            if(this.photos.length > 9){
                this.iterations = 9;
            } else {
                this.iterations = this.photos.length;
            }
            console.log(this.photos.length, this.iterations);
            this.photosStatus = true;
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
    gap: 3px;
    margin: 20px;
    
}
.photos-img {
    justify-self: center;
    width: 300px;
    height: 250px;
}
</style>
