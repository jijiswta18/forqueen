<template>
  <div class="full-page">
    <div class="bg-page">
     <v-container>
      <Nuxt />    
      </v-container>
   
    </div> 
  </div>
  
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>
<style scoped>
  .full-page{
    height: auto;
    min-height: 100vh;
    font-family: 'Kanit', sans-serif;
  }
  .bg-page{
    /* background-color: #a38bc2; */
    background-image: url("~/assets/images/bg2.jpg");
    text-align: center;
    height: auto;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }
  html{
   height: 100%;
  }
  body{
    font-family: 'Kanit', sans-serif;
    height: 100%;
  }
 
</style>
