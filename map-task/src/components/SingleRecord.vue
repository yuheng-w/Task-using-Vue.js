<template>
        {{ record.results[0].formatted_address }} 
</template>

<script>

export default {
    props: ['record']
}
</script>