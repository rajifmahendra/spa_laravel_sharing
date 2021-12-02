<template>
    <div id="main">
        <navbar></navbar>
        <router-view></router-view>
    
        <footer></footer>
       
    </div>

</template>

<script>
    import Navbar from './components/Navbar'
    import Footer from './components/Footer'

    export default {
        name: 'App',
        components: {
            'navbar': Navbar,
            'footer': Footer
        }
    }
</script>
