<template>
  <v-app-bar
    app
    color="indigo"
    dark
  >
    <v-toolbar-title>Application</v-toolbar-title>
    <v-spacer/>
    <div id="buttons">
      <router-link to="/dashboard" class="v-btn icon pa-3">
        <v-icon style="font-size: 30px" class="">
          mdi-home
        </v-icon>
      </router-link>
      <router-link to="/users" class="v-btn icon pa-3">
        <v-icon style="font-size: 30px">
          mdi-account-group
        </v-icon>
      </router-link>
<!--      <v-btn icon>-->
<!--      </v-btn>-->
    </div>
  </v-app-bar>
</template>

<script>
export default {
  name: 'Drawer'
}
</script>

<style scoped>

</style>
