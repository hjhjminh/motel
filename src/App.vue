<template>
    <div id="app">
        <Header/>
        <ComputedMoney style="margin-left: 200px; margin-top: 10px"/>
    </div>
</template>

<script>

    import Header from "@/components/Header";
    import ComputedMoney from "@/components/ComputedMoney";

    export default {
        name: 'App',
        components: {ComputedMoney,Header}
    }
</script>

<style>
    #app {

    }
</style>
