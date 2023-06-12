<template>
    <nav-bar
        v-if="pages.length > 0"
        :pages="pages"
        :nav-link-click="(index) => console.log(index)"
    ></nav-bar>
    <GoogleMap />
</template>


<script>
import NavBar from './components/NavBar.vue';
import GoogleMap from "./components/GoogleMap.vue";

export default {
    components: {
        NavBar,
        GoogleMap
    },
    created () {
        this.getPages();
    },
    data() {
        return {
            activePage: 0,
            pages: [],
            pageTitle: 'Hello, Vue',

            center: {lat: 37.7749, lng: -122.4194}
        };
    },
    methods: {
        changeTheme() {
            let theme = 'light'; 
            if (this.theme == 'light') {
                theme = 'dark';
            }
            this.theme = theme;
        },

        async getPages() {
            let res = await fetch('pages.json');
            let data = await res.json();

            this.pages = data;
        },
    }
}
</script>