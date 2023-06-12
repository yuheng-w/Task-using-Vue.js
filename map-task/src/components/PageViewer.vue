<template>
    <div id="content" class="container">
        <h1> {{ pageTitle }}</h1>
        <div> Current time: {{ time }} </div>

    </div>
</template>

<script>
export default {
    props: ['pageTitle'],

    data () {
        return {
            time: ""
        };
    },
    created() {
        const apiUrl = process.env.VUE_APP_GOOGLE_MAPS_API_KEY;
        console.log(apiUrl);
    },

    mounted() {
        setInterval(() => {
            let date = new Date();
            let utcHours = date.getUTCHours();
            let utcMinutes = date.getUTCMinutes();
            let utcSeconds = date.getUTCSeconds();
            this.time = 'hours: ' + utcHours + ', minites:' + utcMinutes + ', seconds:' +  utcSeconds;
        }, 1000);
    }
}
</script>