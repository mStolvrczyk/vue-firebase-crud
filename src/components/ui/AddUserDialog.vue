<template>
<v-dialog v-model="addUserDialogVisibility">
  <v-container>

    <v-row align="center">
      <v-col cols="8" offset="2" align="center">
        <v-card class="pa-2" color="indigo lighten-2">
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                label="First Name"
                v-model="firstName"
                :error-messages="nameErrors"
                @input="$v.firstName.$touch()"
                @blur="$v.firstName.$touch()"
                required
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
  <AlertDialog
  :alertVisibility.sync:="alertVisibility"
  v-on:closeAlertDialog="closeAlertDialog"
  />
</v-dialog>
</template>

<script>
import db from '../../libs/firebaseInit'
import { required, minLength } from 'vuelidate/lib/validators'
import AlertDialog from './AlertDialog'
export default {
  name: 'AddUserDialog',
  components: { AlertDialog },
  data: () => ({
    firstName: '',
    lastName: '',
    email: '',
    phoneNumber: '',
    address: '',
    alertVisibility: false
  }),
  validations: {
    firstName: {
      required
    },
    lastName: {
      required
    },
    email: {
      required
    },
    phoneNumber: {
      required,
      minLength: minLength(9)
    },
    address: {
      required
    }
  },
  props: {
    addUserDialogVisibility: Boolean
  },
  methods: {
    async saveUser () {
      this.alertVisibility = true
      await db.firestore().collection('users').add({
        first_name: this.firstName,
        last_name: this.lastName,
        email: this.email,
        phone_number: this.phoneNumber,
        address: this.address
      })
      this.firstName = null
      this.lastName = null
      this.email = null
      this.phoneNumber = null
      this.address = null
      this.$emit('updateAddUserDialogVisibility', false)
    },
    closeAlertDialog (value) {
      this.alertVisibility = value
    }
  },
  computed: {
    nameErrors () {
      const errors = []
      if (!this.$v.firstName.$dirty) return errors
      !this.$v.firstName.required && errors.push('First name is required.')
      return errors
    }
  }
}
</script>

<style scoped>

</style>
