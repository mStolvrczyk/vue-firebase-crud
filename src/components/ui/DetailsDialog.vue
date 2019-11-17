<template>
      <v-row justify="center">
  <v-dialog v-model="detailsDialogVisibility" fullscreen hide-overlay transition="dialog-bottom-transition">
          <v-card color="white" class="pa-5">
            <v-row>
              <v-col cols="5">
                <v-card color="blue-grey lighten-4" class="pa-2">
                      <v-card color="indigo darken-1" class="white--text pa-3 font-weight-bold" align="center">
                        USER DETAILS
                      </v-card>
                  <v-row>
                    <v-col cols="6">
                      <v-card align="center" color="indigo darken-1" class="pa-1 white--text font-weight-bold">First Name</v-card>
                    </v-col>
                    <v-col cols="6">
                      <div v-if="editing" class="pa-1">
                        <input style="background-color: white" type="text" v-model="userDetails.firstName">
                      </div>
                      <div v-else class="pa-1 grey--text font-weight-bold">{{userDetails.firstName}}</div>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="6">
                      <v-card align="center" color="indigo darken-1" class="pa-1 white--text font-weight-bold">Last name</v-card>
                    </v-col>
                    <v-col cols="6">
                      <div v-if="editing" class="pa-1">
                        <input style="background-color: white" type="text" v-model="userDetails.lastName">
                      </div>
                      <div v-else class="pa-1 grey--text font-weight-bold">{{userDetails.lastName}}</div>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="6">
                      <v-card align="center" color="indigo darken-1" class="pa-1 white--text font-weight-bold">Email</v-card>
                    </v-col>
                    <v-col cols="6">
                      <div v-if="editing" class="pa-1">
                        <input style="background-color: white" type="text" v-model="userDetails.email">
                      </div>
                      <div v-else class="pa-1 grey--text font-weight-bold">{{userDetails.email}}</div>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="6">
                      <v-card align="center" color="indigo darken-1" class="pa-1 white--text font-weight-bold">Phone number</v-card>
                    </v-col>
                    <v-col cols="6">
                      <div v-if="editing" class="pa-1">
                        <input style="background-color: white" type="text" v-model="userDetails.phoneNumber">
                      </div>
                      <div v-else class="pa-1 grey--text font-weight-bold">{{userDetails.phoneNumber}}</div>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="6">
                      <v-card align="center" color="indigo darken-1" class="pa-1 white--text font-weight-bold">Address</v-card>
                    </v-col>
                    <v-col cols="6">
                      <div v-if="editing === true" class="pa-1">
                        <input style="background-color: white" type="text" v-model="userDetails.address">
                      </div>
                      <div v-if="editing === false" class="pa-1 grey--text font-weight-bold">{{userDetails.address}}</div>
                    </v-col>
                  </v-row>
                </v-card>
                <v-row>
                  <v-col>
                    <v-btn @click="edit" color="blue-grey lighten-1" class="white--text mx-2">Edit</v-btn>
                  </v-col>
<!--                  <v-spacer></v-spacer>-->
                  <v-col v-if="editing">
                    <div align="right">
                      <v-btn @click="back" color="red" class="white--text mx-2">Back</v-btn>
                      <v-btn @click="editing = true" color="green" class="white--text mx-2">Save</v-btn>
                    </div>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
          </v-card>
  </v-dialog>
      </v-row>
</template>

<script>
import db from '../../libs/firebaseInit'
export default {
  name: 'DetailsDialog',
  data: () => ({
    editing: false,
    userDetailsHolder: null
  }),
  props: {
    userDetails: Object,
    detailsDialogVisibility: Boolean
  },
  methods: {
    edit () {
      this.editing = true
      this.userDetailsHolder = this.userDetails
    },
    back () {
      this.editing = false
      this.userDetails = this.userDetailsHolder
    },
    closeDialog () {
      this.$emit('updateDetailsDialogVisibility', false)
    },
    updateData () {

    }
  }
}
</script>

<style scoped>
.container {
  max-width: 100%;
}
</style>
