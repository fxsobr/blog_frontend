<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container">
            <router-link class="navbar-brand" to="/">
                <img src="../assets/logo.png" style="width: 30px; height: 30px">
            </router-link>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ml-auto">
                    <li class="nav-item" v-if="!isAuth">
                        <router-link class="nav-link" to="/login">Login</router-link>
                    </li>
                    <li class="nav-item" v-if="!isAuth">
                        <router-link class="nav-link" to="/signup">Signup</router-link>
                    </li>
                    <li class="nav-item dropdown" v-if="isAuth">
                        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            Hey {{ $root.auth.user.name }}
                        </a>
                        <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                            <a class="dropdown-item" @click="logout()" href="#">Logout</a>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
    import Axios from 'axios';
    import config from "@/config";
    export default {
        mounted() {
            console.log(this.$root)
        },
        computed: {
            isAuth() {
                return this.$root.auth.user;
            }
        },
        methods: {
            logout() {
                Axios.post(`${config.apiUrl}/logout`, null, {
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization' : `Bearer ${this.$root.auth.access_token}`,
                    }
                }).then(response => {
                    localStorage.removeItem('auth');
                    this.$root.auth = {};
                    this.$noty.warning(response.data.message);
                    console.log(response.data.message)
                }).catch(({response}) => {
                    this.$noty.error(response.data.message);
                })
            }
        }
    }
</script>

<style scoped>

</style>