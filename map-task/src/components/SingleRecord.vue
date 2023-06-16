<template>
        <!-- Display the address -->
        {{ record.formatted_address }} 
</template>

<script>

export default {
    props: ['record']
}
</script>