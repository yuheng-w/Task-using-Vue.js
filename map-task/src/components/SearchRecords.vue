<template>
    <div>
        <ul>
            <li v-for="(record, index) in records" :key="index">
                <a> {{ record.results[0].formatted_address }} </a>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: ['records']
}
</script>