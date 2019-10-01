<template>
    <div class="container my-5">
        <div class="row">
            <div class="col-md-10 offset-md-1">
                <div class="card" v-if="!loading">
                    <img height="420px" :src="article.image_url" alt="" class="card-img-top">
                    <div class="card-body">
                        <h1 class="card-title text-center my-3">{{ article.title }}</h1>
                        <div class="article-content" v-html="article.description"></div>
                        <div class="comments my-4">
                            <vue-disqus shortname="community-blog" :identifier="article.slug" :url="url"></vue-disqus>
                        </div>
                    </div>
                </div>
                <div class="loader text-center" v-else>
                    <i class="fas fa-8x fa-spin fa-spinner"></i>
                </div>
            </div>
        </div>
    </div>
    </template>

<script>
    import Axios from 'axios';
    import config from "@/config";
    export default {

        mounted() {
            this.getArticle()
        },
        data(){
          return {
              article: {},
              loading: true,
              url: window.location.href
          }
        },
        methods: {
            getArticle() {
                Axios.get(`${config.apiUrl}/article`, {
                    headers: {"Authorization" : `Bearer ${this.$root.auth.access_token}`},
                    params:{
                        "id": this.$route.params.id
                    }
                }).then(response => {
                    console.log(response);
                    this.loading = false;
                    this.article = response.data
                }).catch(({response}) => {
                    console.log(response)
                })
            },
        }
    }
</script>

<style scoped>

</style>
