<template>
    <a> {{ record.results[0].formatted_address }} </a>
</template>

<script>

export default {
    props: ['record']
}
</script>