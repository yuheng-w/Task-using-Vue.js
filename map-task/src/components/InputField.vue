<template>
    <form>
        <input
            type="text"
            v-model="content"
        />
        <button
            @click.prevent="submitFunction({content})"
        > Submit </button>
    </form>
</template>

<script>
export default {
    props: ['submitFunction'],
    data() {
        return {
            content: ''
        }
    }
}
</script>