<template>
    <div class="row">
        <div class="col-md-8 offset-md-2" v-if="articles" v-for="article in articles" :key="article.id">
            <Article :article="article"/>
        </div>
    </div>
</template>

<script>
    import Axios from 'axios';
    import config from "@/config";
    import Article from "@/components/Article";
    export default {
        components: {
            Article
        },
        mounted() {
            this.getArticles();
        },

        data() {
            return {
                articles: {}
            }
        },
        methods: {
            getArticles() {
                Axios.get(`${config.apiUrl}/articles`, {
                    headers: {"Authorization" : `Bearer ${this.$root.auth.access_token}`}
                }).then(response => {
                    console.log(response);
                    if (response.status === 401) {
                        console.log('oi',response);
                    } else {
                        this.articles = response.data.articles
                    }
                }).catch(({response}) => {
                    if (response.status === 401) {
                        this.refreshToken()
                    }
                })
            },
            refreshToken() {
                Axios.post(`${config.apiUrl}/refresh`, null, {
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization' : `Bearer ${this.$root.auth.refresh_token}`,
                    }
                }).then(response => {
                    this.$noty.success("Access Token was Refreshed");
                    this.addToLocalStorageObject('auth', 'access_token', response.data.access_token);
                    console.log(response)
                }).catch(({response}) => {
                    console.log('erro', response)
                })
            },
            addToLocalStorageObject (name, key, value) {
                let existing = localStorage.getItem(name);
                existing = existing ? JSON.parse(existing) : {};
                existing[key] = value;
                localStorage.setItem('auth', JSON.stringify(existing));
            },
        }
    }
</script>

<style scoped>

</style>