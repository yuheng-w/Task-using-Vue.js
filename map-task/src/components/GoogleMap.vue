<template>
    <!-- Map -->
    <GoogleMap 
        :api-key="apiKey"
        style="width: 100%; height: 500px" :center="center" :zoom="5"
    >
        <div>
            <input-field :submit-function="submitFunction"></input-field>
            <div>
                <button class="btn btn-success mb-2" @click.prevent="getCurrentLocation()"> Current Location </button>
            </div>
            <button class="btn btn-primary mb-2" @click.prevent="deleteItems()"> Delete </button>
            <SearchedRecords :records="records" :selectedCheckboxes="selectedCheckboxes" :key="records"></SearchedRecords>
        </div>
    </GoogleMap>

</template>

<script>
import { defineComponent } from "vue";
import { GoogleMap } from "vue3-google-map";
import InputField from "./InputField.vue";
import SearchedRecords from "./SearchedRecords.vue";

export default defineComponent({
    components: { GoogleMap, SearchedRecords, InputField },
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
                    const formatted_address = res.results[0].formatted_address;
                    const location = { lat: res.results[0].geometry.location.lat, lng: res.results[0].geometry.location.lng}
                    const time_zone = "";
                    const local_time = "";
                    const record = {
                        formatted_address: formatted_address,
                        location: location,
                        time_zone: time_zone,
                        local_time: local_time
                    }
                    this.center = location;
                    this.records.unshift(record);
                    this.computeTime();
                }
            });
        },
        selectedCheckboxes(seletectedItems) {
            this.seletectedItems = seletectedItems;
        },
        deleteItems() {
            this.records = this.records.filter((record) => !this.seletectedItems.includes(record));
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
        },

        async fetchTimeZone() {
                const lat = this.records[0].location.lat;
                const lng = this.records[0].location.lng;
                const url = `https://maps.googleapis.com/maps/api/timezone/json?location=${lat},${lng}&timestamp=${Math.floor(Date.now() / 1000)}&key=${this.apiKey}`;
                const res = await fetch(url);
                return res.json();
        },
        computeTime() {
            if (this.records.length > 0) {
                // delete time zone and local time for the second row
                if (this.records.length > 1) {
                    for (let i = 1; i < this.records.length; i ++) {
                        this.records[i].time_zone = "";
                        this.records[i].local_time = "";
                    }
                }
                this.fetchTimeZone().then((res) => {
                    const timezone = res.timeZoneName; 
                    const timestamp = Math.floor(Date.now() / 1000);
                    const localDate = new Date((timestamp + res.dstOffset + res.rawOffset) * 1000);
                    const localTime = localDate.toUTCString();
                    this.records[0].time_zone = timezone;
                    this.records[0].local_time = localTime;
                });
            }
        },
    }
    
});
</script>

