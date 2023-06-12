<template>
    <form>
        <input
            type="text"
            v-model="address"
        />
        <button
            @click.prevent="submitFunction(address)"
        > Submit </button>
    </form>
</template>

<script>
export default {
    props: ['submitFunction'],
    data() {
        return {
            address: ''
        }
    }
}
</script>