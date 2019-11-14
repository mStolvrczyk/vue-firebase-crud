<template>
  <v-dialog v-model="detailsDialogVisibility">
    <v-container>
      <v-row>
        <v-col cols="12">
          <v-card height="500px" color="indigo lighten-2" class="pa-3">
            <v-row>
              <v-col cols="3">
                <v-card color="indigo lighten-1" align="left">
                  <v-card-text class="white--text font-weight-bold">First Name: {{userDetails.firstName}}</v-card-text>
                  <v-card-text class="white--text font-weight-bold">Last Name: {{userDetails.lastName}}</v-card-text>
                  <v-card-text class="white--text font-weight-bold">Email: {{userDetails.email}}</v-card-text>
                  <v-card-text class="white--text font-weight-bold">Phone: {{userDetails.phoneNumber}}</v-card-text>
                  <v-card-text class="white--text font-weight-bold">Address: {{userDetails.address}}</v-card-text>
                </v-card>
              </v-col>
              <v-col cols="8"></v-col>
            </v-row>
<!--            <v-btn @click="closeDialog">-->
<!--            </v-btn>-->
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-dialog>
</template>

<script>
export default {
  name: 'DetailsDialog',
  props: {
    userDetails: Object,
    detailsDialogVisibility: Boolean
  },
  methods: {
    closeDialog () {
      this.$emit('updateDetailsDialogVisibility', false)
    }
  }
}
</script>

<style scoped>

</style>
