<template>
    <a> {{ record.results[0].formatted_address }} </a>
    <Marker :options="{ position: record.results[0].geometry.location }" />
</template>

<script>
import { Marker } from "vue3-google-map";

export default {
    components: { Marker },
    props: ['record']
}
</script>