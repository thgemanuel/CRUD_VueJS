<template>
  <div>
    <Navbar :logo="logosrc" :alt="appname"/>
    <router-view />
    <Footer />
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue';
import Footer from './components/Footer.vue';

export default {
  components:{
    Navbar,
    Footer
  },
  data(){
    return{
      logosrc: "/img/logo.png",
      appname: "Make your burguer!",
    }
  }
}
</script>

<style>
*{
  font-family: Helvetica;
  padding: 0;
  margin: 0;
  /* o border box serve para os inputs nao 
    passarem da largura max da div pai */
  box-sizing: border-box;
}

.main-container{
  margin: 50px;
  /* altura minima para o corpo do site  */
  min-height: 250px;
}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
