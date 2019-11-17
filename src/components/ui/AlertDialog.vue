<template>
  <v-dialog v-model="alertVisibility">
    <v-container>
      <v-row align="center">
        <v-col cols="4" offset="4">
          <v-card color="indigo lighten-2" align="center">
            <v-card-text class="white--text" style="font-weight: bold; font-size: 20px; font-style: oblique">Please,
        fill in all fields</v-card-text>
            <v-row>
              <v-col cols="4" offset="4">
                <v-btn @click="closeDialog" color="green" class="white--text">OK</v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-dialog>
</template>

<script>
export default {
  name: 'AlertDialog',
  props: {
    alertVisibility: Boolean
  },
  methods: {
    closeDialog () {
      this.$emit('closeAlertDialog', false)
    }
  }
}
</script>

<style scoped>

</style>
