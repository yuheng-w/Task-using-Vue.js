<template>
    <div>
        <ul>
            <li v-for="(record, index) in records" :key="index">
                <SingleRecord :record="record"></SingleRecord>
                <div v-if="index === 0"> {{ timezone }} <br> {{ localTime }}</div>
            </li>
        </ul>
    </div>
</template>

<script>
import SingleRecord from './SingleRecord.vue';

export default {
    components: { SingleRecord },
    props: ['records'],
    data() {
        return {
            apiKey: process.env.VUE_APP_GOOGLE_MAPS_API_KEY,
            timezone: "",
            localTime: ""
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
        }
    }
}
</script>