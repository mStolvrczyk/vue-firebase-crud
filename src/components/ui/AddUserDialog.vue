<template>
<v-dialog v-model="addUserDialogVisibility">
  <v-container>
    <v-row align="center">
      <v-col cols="8" offset="2" align="center">
        <v-card class="pa-2" color="indigo lighten-2">
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                v-model="firstName"
                label="First Name"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                v-model="lastName"
                label="Last Name"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                v-model="email"
                label="Email"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                v-model="phoneNumber"
                label="Phone Number"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                v-model="address"
                label="Address"
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col>
              <v-btn @click="saveUser" color="green" class="white--text">Add user</v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</v-dialog>
</template>

<script>
import db from '../../libs/firebaseInit'
export default {
  name: 'AddEmployeeDialog',
  data: () => ({
    // userDetails: {
    id: null,
    firstName: null,
    lastName: null,
    email: null,
    phoneNumber: null,
    address: null
    // }
  }),
  props: {
    addUserDialogVisibility: Boolean
  },
  methods: {
    saveUser () {
      db.firestore().collection('users').add({
        id: '',
        first_name: this.firstName,
        last_name: this.lastName,
        email: this.email,
        phone_number: this.phoneNumber,
        address: this.address
      })
      this.$emit('updateAddUserDialogVisibility', false)
    }
  }
}
</script>

<style scoped>

</style>
