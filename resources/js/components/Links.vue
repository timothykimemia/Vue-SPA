<template>
    <div>
        <section class="text-center mb-5">
            <div class="container">
                <h1 class="jumbotron-heading"></h1>
                <p class="lead text-muted"></p>
                <span v-for="item in navbar_items">
                    <router-link class="mt-2 mr-1 btn btn-secondary" exact :to="item.to">
                        <i :class="`fa fa-${item.icon}`"></i>
                        {{ item.name }}
                    </router-link>
                </span>
                <router-link v-show="!show" class="mt-2 mr-1 btn btn-secondary" exact to="/login">
                    <i class="fas fa-sign-in-alt"></i>
                    {{ $t('buttons.login')}}
                </router-link>
                <router-link v-show="isAdmin" class="mt-2 mr-1 btn btn-secondary" exact :to="{ name: 'links' }">
                    {{ $t('generals.links') }}
                </router-link>
                <router-link v-show="isAdmin" class="mt-2 mr-1 btn btn-secondary" exact :to="{ name: 'permissions.create' }">
                    {{ $t('buttons.new_permission')}}
                </router-link>
            </div>
        </section>
    </div>
</template>

<script>
export default {
    data() {
        return {
            navbar_items: [],
            show: this.$store.state.show_content,
        }
    },
    computed: {
        isAdmin() {
            return this.$store.state.currentUser.isAdmin;
        },
    },
    mounted() {
        if (this.show) {
            this.loadLinks();
        }

    },
    methods: {
        loadLinks() {
            axios.get('/api/v1/links/get-url').then(response => {
                this.navbar_items = response.data.url
            })
        }
    }
}
</script>

<style scoped></style>
