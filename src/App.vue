<template>
  <v-app>
    <Navbar/>
    <v-content>
      <v-container class="fill-height">
        <router-view/>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

import Navbar from './components/ui/Navbar'
// import View from './components/ui/View'
export default {
  name: 'App',
  components: { Navbar },
  data: () => ({
    //
  })
}
</script>
<style>
  #app {
    background-color: #C5CAE9 ;
  }
</style>
