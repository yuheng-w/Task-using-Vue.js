<template>
    <nav 
        :class="[`navbar-${theme}`, `bg-${theme}`, 'navbar', 'navbar-expand-lg']"
    >
        <div class="container fluid">
            <a class="navbar-brand" href="#">Map Search</a>
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                <li v-for="(page, index) in pages" :key="index" class="nav-item">
                    <a 
                        class="nav-link" 
                        aria-current="page" 
                        href="#"
                        @click.prevent="navLinkClick(index)"
                        >
                        {{ page.title }}
                    </a>
                </li>
            </ul>
            <form class="d-flex">
                <button
                    class="btn btn-primary"
                    @click.prevent="changeTheme()"
                >Change Theme</button>
            </form>
        </div>
    </nav>
</template>

<script>
export default {
    props: ['pages', 'nav-link-click'],

    data() {
        return {
            theme: 'light'
        }
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

<style scoped>
.nav-item {
    filter: drop-shadow(4px 4px 4px rgba(0, 0, 0, 0.25));
}
</style>