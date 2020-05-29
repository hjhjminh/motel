<template>
    <div id="app">
        <Header/>
        <ComputedMoney style="margin-left: 350px; margin-top: 50px"/>
    </div>
</template>

<script>

    import Header from "@/components/Header";
    import ComputedMoney from "@/components/ComputedMoney";

    export default {
        name: 'App',
        components: {ComputedMoney, Header}
    }
</script>

<style>
    #app {

    }
</style>
