<template>
    <div>
        <ul>
            <div v-for="(record, index) in records" :key="index">
                <Marker :options="{ position: record.results[0].geometry.location }" />
            </div>
            <li v-for="(record, index) in displayedItems" :key="index">
                <SingleRecord :record="record"></SingleRecord>
                <div v-if="start + index === 0"> {{ timezone }} <br> {{ localTime }}</div>
            </li>
        </ul>
        <button @click="previousPage" :disabled="currentPage === 1">Previous</button>
        <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
        
        <div>
            <span v-for="pageNumber in totalPages" :key="pageNumber">
                <button @click="goToPage(pageNumber)">{{ pageNumber }}</button>
            </span>
        </div>
    </div>
</template>

<script>
import SingleRecord from './SingleRecord.vue';
import { Marker } from "vue3-google-map";


export default {
    components: { SingleRecord, Marker },
    props: ['records'],
    data() {
        return {
            apiKey: process.env.VUE_APP_GOOGLE_MAPS_API_KEY,
            timezone: "",
            localTime: "",
            currentPage: 1,
            itemsPerPage: 10,
        }
    },

    mounted() {
        this.computeTime();
        setInterval(this.computeTime, 1000);
    },

    methods: {
        async fetchTimeZone() {
                let lat = this.records[0].results[0].geometry.location.lat;
                let lng = this.records[0].results[0].geometry.location.lng;
                const url = `https://maps.googleapis.com/maps/api/timezone/json?location=${lat},${lng}&timestamp=${Math.floor(Date.now() / 1000)}&key=${this.apiKey}`;
                const res = await fetch(url);
                return res.json();
        },
        computeTime() {
            if (this.records.length > 0) {
                this.fetchTimeZone().then((res) => {
                    const timezone = "Time zone: " + res.timeZoneName; 
                    const timestamp = Math.floor(Date.now() / 1000);
                    const localDate = new Date((timestamp + res.dstOffset + res.rawOffset) * 1000);
                    const localTime = "Local time: " + localDate.toUTCString();
                    this.timezone = timezone;
                    this.localTime = localTime;
                });
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
            this.currentPage++;
            }
        },
        previousPage() {
            if (this.currentPage > 1) {
            this.currentPage--;
            }
        },
        goToPage(pageNumber) {
            if (pageNumber >= 1 && pageNumber <= this.totalPages) {
            this.currentPage = pageNumber;
            }
        }
    },

    computed: {
        start() {
            return (this.currentPage - 1) * this.itemsPerPage;
        },
        end() {
            return this.start + this.itemsPerPage;
        },
        displayedItems() {
            return this.records.slice(this.start, this.end);
        },
        totalPages() {
            return Math.ceil(this.records.length / this.itemsPerPage);
        }
    },
}
</script>