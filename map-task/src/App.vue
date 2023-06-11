<template>

    <nav-bar
        v-if="pages.length > 0"
        :pages="pages"
        :nav-link-click="(index) => console.log(index)"
    ></nav-bar>
    <div class="container">
        <page-viewer :page-title="pageTitle"></page-viewer>
        <input-field :submit-function="submitFunction"></input-field>
    </div>
</template>


<script>
import PageViewer from './components/PageViewer.vue';
import NavBar from './components/NavBar.vue';
import InputField from './components/InputField.vue';

export default {
    components: {
        PageViewer,
        NavBar,
        InputField
    },
    created () {
        this.getPages();
    },
    data() {
        return {
            activePage: 0,
            pages: [],
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
        },

        async getPages() {
            let res = await fetch('pages.json');
            let data = await res.json();

            this.pages = data;
        },

        submitFunction(content) {
            console.log(content);
        }
    }
}
</script>