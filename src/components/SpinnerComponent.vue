<template>
    <div class="container d-flex flex-wrap justify-content-center">
        <p class="text-center w-100">{{text}}</p>
        <div class="spinner-border " role="status">
            <span class="visually-hidden"></span>
        </div>
    </div>
</template>

<script>
export default {
    name:'spinner',
    props:{
        text:String,
    }

}
</script>

<style>

</style>