<template>
      <v-row justify="center">
  <v-dialog v-model="detailsDialogVisibility" fullscreen hide-overlay transition="dialog-bottom-transition">
<!--    <v-container>-->
<!--        <v-col cols="10" offset="1">-->
          <v-card color="white" class="pa-5">
            <v-row>
              <v-col cols="3">
                <v-card color="indigo lighten-3" width="300px">
                  <v-row>
                    <v-col cols="12">
                      <v-card>
                        USER DETAILS
                      </v-card>
                    </v-col>
                  </v-row>
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
<!--        </v-col>-->
<!--    </v-container>-->
  </v-dialog>
      </v-row>
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
.container {
  max-width: 100%;
}
</style>
