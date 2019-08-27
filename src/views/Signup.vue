<template>
    <div class="row my-5">
        <div class="col-md-6 offset-md-3">
            <div class="card">

                <h3 class="text-center my-4">Signup</h3>

                <div class="card-body">
                    <div class="form-group">
                        <input v-model="name" type="text" placeholder="Name" class="form-control">
                    </div>
                    <div class="form-group">
                        <input v-model="email" type="text" placeholder="Email" class="form-control">
                    </div>
                    <div class="form-group">
                        <input v-model="password" type="password" placeholder="Password" class="form-control">
                    </div>
                    <div class="form-group text-center">
                        <button @click="registerUser()" :disabled="loading" class="btn btn-success form-control">
                            <i class="fas fa-spin fa-spinner" v-if="loading"></i>
                            {{ loading ? '' : 'Register' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Axios from 'axios';
    import config from "@/config";
    export default {
        beforeRouteEnter(to, from, next) {
            if (localStorage.getItem("auth")) {
                return next({ path: "/"});
            }

            next();

        },
        data() {
            return {
                name: '',
                email: '',
                password: '',
                loading: false
            }
        },
        methods: {
            registerUser(){
                this.loading = true;
                console.log(this.name, this.email, this.password);
                Axios.post(`${config.apiUrl}/register`, {
                    name: this.name,
                    email: this.email,
                    password: this.password
                }).then(response =>{
                    this.loading = false;
                    console.log(response);
                    this.$noty.success("Registered Sucessfully!");
                    this.$router.push("/")
                }).catch(({ response }) => {
                    this.loading = false;
                    this.$noty.error("Oops, something went wrong!");
                    console.log(response)
                })
            }
        }
    }
</script>

<style scoped>

</style>