<template>
  <NavBar :logo="logoSrc" :alt="altM"/>
  <router-view/>
  <Footer />
</template>

<script>
import NavBar from './components/NavBar.vue';

import Footer from './components/Footer.vue';

export default{
  components:{
    NavBar,
    Footer
  },
  data(){
    return{
      logoSrc: "/imgs/logo.png",
      altM: "Your burguer"
    }
  }
}

</script>
<style>
*{
  font-family: Arial, Helvetica, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.main-container{
  margin: 50px;
  min-height: 400px;
}
h1{
  text-align: center;
  font-size: 42px;
  color: #222;
  margin-bottom: 30px;
}
</style>
