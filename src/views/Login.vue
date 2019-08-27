<template>
    <div class="row my-5">
        <div class="col-md-6 offset-md-3">
            <div class="card">

                <h3 class="text-center my-4">Login</h3>

                <div class="card-body">
                    <div class="form-group">
                        <input v-bind:class="{'is-invalid': errors.email}" v-model="email" type="text" placeholder="email" class="form-control">
                        <div class="errors" v-if="errors.email">
                            <small class="text-danger" :key="index+email" v-for="(error, index) in errors.email">{{ error }}</small>
                        </div>
                    </div>
                    <div class="form-group">
                        <input v-bind:class="{'is-invalid': errors.password}" v-model="password" type="password" placeholder="password" class="form-control">
                        <div class="errors" v-if="errors.password">
                            <small class="text-danger" :key="index+password" v-for="(error, index) in errors.password">{{ error }}</small>
                        </div>
                    </div>
                    <div class="form-group text-center">
                        <button @click="loginUser()" :disabled="loading" class="btn btn-success form-control">
                            <i class="fas fa-spin fa-spinner" v-if="loading"></i>
                            {{ loading ? '' : 'Login' }}
                        </button>
                    </div>
                    <div class="errors text-center" v-if="errors">
                        <small class="text-danger text-center" :key="index+password" v-for="(error, index) in errors">{{ error }}</small>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Axios from 'axios';
    export default {
        data() {
            return {
               email: '',
               password: '',
               errors: {},
               loading: false
            }
        },
        methods: {
            loginUser() {
                this.loading = true;
                Axios.post('http://localhost:5000/auth', {
                    email: this.email,
                    password: this.password
                }).then((response) => {
                    this.loading = false;
                    this.$root.auth = response.data;
                    localStorage.setItem('auth', JSON.stringify(response.data));
                    this.$noty.success("Sucessfully authenticated!");
                    this.$router.push("/")
                }).catch(({response}) => {
                    this.loading = false;
                    this.$noty.error("Oops, something went wrong!");
                    console.log(response);
                    this.errors = response.data.message;
                })
            }
        }
    }
</script>

<style scoped>

</style>