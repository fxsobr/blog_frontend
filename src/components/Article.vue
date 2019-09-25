<template>
    <div class="card my-3">
        <div class="card-header font-weight-bold">{{article.title}}</div>
        <div class="card-body">
            <div class="my-3" v-html="article.description.substring(0, 50)"></div>
            <button class="btn btn-primary">Read more</button>
        </div>
    </div>
</template>

<script>
    export default {
        props: ["article"]
    }
</script>

<style scoped>

</style>