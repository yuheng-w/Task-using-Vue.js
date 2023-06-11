<template>
    <nav-bar
        :pages="pages"
        :nav-link-click="(index) => console.log(index)"
    ></nav-bar>

    <page-viewer :page-title="pageTitle"></page-viewer>
</template>


<script>
import PageViewer from './components/PageViewer.vue';
import NavBar from './components/NavBar.vue';

export default {
    components: {
        PageViewer,
        NavBar
    },

    data() {
        return {
            activePage: 0,
            pages: [
                {
                    title: 'Home',
                    content: 'Home'
                },
                {
                    title: 'About',
                    content: 'About'
                },
                {
                    title: 'Contact',
                    content: 'Contact'
                }
            ],
            pageTitle: 'Hello, Vue'
        };
    },
    methods: {
        changeTheme() {
            let theme = 'light'; 
            if (this.theme == 'light') {
                theme = 'dark';
            }
            this.theme = theme;
        }
    }
}
</script>