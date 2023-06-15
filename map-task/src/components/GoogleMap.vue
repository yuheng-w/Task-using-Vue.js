<template>
    <!-- Map -->
    <GoogleMap 
        :api-key="apiKey"
        style="width: 100%; height: 500px" :center="center" :zoom="5"
    >
        <div>
            <input-field :submit-function="submitFunction"></input-field>
            <button @click.prevent="deleteItems()"> Delete </button>
            <button @click.prevent="getCurrentLocation()"> Current Location </button>
            <SearchedRecords :records="records" :selectedCheckboxes="selectedCheckboxes" :key="records"></SearchedRecords>
        </div>
        <!-- <button @click.prevent="getCurrentLocation()"> Current Location </button> -->
    </GoogleMap>

</template>

<script>
import { defineComponent } from "vue";
import { GoogleMap } from "vue3-google-map";
import InputField from "./InputField.vue";
import SearchedRecords from "./SearchedRecords.vue";

export default defineComponent({
    components: { GoogleMap, InputField, SearchedRecords },
    data () {
        return {
            apiKey: process.env.VUE_APP_GOOGLE_MAPS_API_KEY,
            center: { lat: 43.6515, lng: -79.3835 },
            records: [],
            seletectedItems:[]
        }
    },
    methods: {
        async findLocation(address) {
            const apiKey = this.apiKey;
            const url = `https://maps.googleapis.com/maps/api/geocode/json?address=${encodeURIComponent(address)}&key=${apiKey}`;
            const res = await fetch(url);
            return res.json();
        },
        submitFunction(address) {
            this.findLocation(address).then((res) => {
                console.log(res);
                if (res.status === 'OK') {
                    let location = res.results[0].geometry.location;
                    this.center = location;
                    this.records.push(res);
                }
            });
        },
        selectedCheckboxes(seletectedItems) {
            this.seletectedItems = seletectedItems;
        },
        deleteItems() {
            this.records = this.records.filter((_, index) => !this.seletectedItems.includes(index));
        },
        getCurrentLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(showPosition);
            } else {
                console.log("Geolocation is not supported by this browser.");
            }

            function showPosition(position) {
                var latitude = position.coords.latitude;
                var longitude = position.coords.longitude;
                alert("Current Location:" + "\n" + "Latitude: " + latitude + "\n" + "Longitude: " + longitude)
            }
        }
    }
    
});
</script>

