<template>
    <GoogleMap 
        :api-key="apiKey"
        style="width: 100%; height: 500px" :center="center" :zoom="15"
    >
        <div>
            <input-field :submit-function="submitFunction"></input-field>
            <button @click.prevent="deleteRecords"> Delete </button>
            <SearchedRecords :records="records"></SearchedRecords>
        </div>
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
            records: []
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
                    this.records.unshift(res);
                    // let temp = this.records;
                    // this.records = [res, ...temp];
                }
            });
        },
        deleteRecords() {
            this.records = [];
        }
    }
    
});
</script>

