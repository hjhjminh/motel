<template>
    <div id="header">
            <img @click="reload" src="@/assets/house.jpg" class="imgHomePage"/>
    </div>
</template>

<script>
    export default {
        name: "Header",
        data() {
            return {
                homePage: 'localhost:8080'
            }
        },
        methods: {
            reload() {
                location.reload();
            }
        }
    }
</script>

<style scoped>
    #header {
    }
    .imgHomePage {
        width: 150px;
        height: 150px;
        margin-left: 150px;
        margin-top: 5px;
    }
</style>