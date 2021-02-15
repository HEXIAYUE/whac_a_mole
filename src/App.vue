<template>
    <div id="app">
        <home class="home"></home>
        <Footer class="footer"></Footer>
    </div>
</template>

<script>
import home from './components/home';
import footer from './components/footer';
export default {
    name: 'App',
    components: {
        home,
        Footer: footer
    }
};
</script>

<style lang="scss">
html,body{
    max-height: 100vh;
    margin:0;
    padding:0;
}
#app {
    height: 100vh;
    max-width: 100vw;
    background-image: url('./assets/1.jpg');
    background-size: 100% 100%;
    background-attachment: fixe;
    position: relative;
    overflow: hidden;
    .home {
        padding-top: 30px;
    }

    .footer {
        position: absolute;
        bottom: 5%;
    }
}
</style>
