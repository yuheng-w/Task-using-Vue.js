<template>
    <div>
        <ul>
            <!-- Marker for all records -->
            <div v-for="(record, index) in records" :key="index">
                <Marker :options="{ position: record.location }" />
            </div>

            <!-- Records in current page -->
            <!-- <li v-for="(record, index) in displayedItems" :key="index">
                <input type="checkbox" @change="selectedCheckboxes(record)">
                <SingleRecord :record="record"></SingleRecord>
                <div v-if="start + index === this.records.length - 1"> {{ timezone }} <br> {{ localTime }}</div>
            </li> -->
        </ul>

        <!-- Pagination -->
        <!-- <button @click="previousPage" :disabled="currentPage === 1">Previous</button>
        <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
        <div>
            <span v-for="pageNumber in totalPages" :key="pageNumber">
                <button @click="goToPage(pageNumber)">{{ pageNumber }}</button>
            </span>
        </div> -->

        <!-- Record table -->
        <el-table :data="displayedItems" stripe style="width: 100%" @selection-change="selectedCheckboxes">
            <el-table-column type="selection" width="55" />
            <el-table-column prop="formatted_address" label="Formatted address" width="180" />
            <el-table-column prop="location.lat" label="Latitude" width="180" />
            <el-table-column prop="location.lng" label="Longitude" width="180" />
            <el-table-column prop="time_zone" label="Time zone" width="180" />
            <el-table-column prop="local_time" label="Local Time" />
        </el-table>
        <!-- Pagination -->
        <el-pagination 
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            layout="sizes, prev, pager, next, jumper" 
            :total="records.length" 
            :page-size="pageSize"
            :current-page="currentPage"
        />
    </div>
</template>

<script>
import { Marker } from "vue3-google-map";


export default {
    components: { Marker },
    props: {
        records: {
            type: Array,
            required: true
        },
        selectedCheckboxes: {
            type: Function,
            required: true
        }
    },

    data() {
        return {
            apiKey: process.env.VUE_APP_GOOGLE_MAPS_API_KEY,
            currentPage: 1,
            pageSize: 10,
        }
    },
    methods: {
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
        },
        handleSizeChange(pageSize) {
            this.pageSize = pageSize;
        },
        handleCurrentChange(currentPage) {
            this.currentPage = currentPage;
        },
    },

    computed: {
        start() {
            return (this.currentPage - 1) * this.pageSize;
        },
        end() {
            return this.start + this.pageSize;
        },
        displayedItems() {
            return this.records.slice(this.start, this.end);
        },
        totalPages() {
            return Math.ceil(this.records.length / this.pageSize);
        }
    },
}
</script>