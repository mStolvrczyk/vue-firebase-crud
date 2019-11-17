<template>
  <v-row justify="center">
    <v-dialog v-model="addUserDialogVisibility" max-width="600px">
    <!--  <v-container>-->

    <!--    <v-row align="center">-->
    <!--      <v-col cols="8" offset="2" align="center">-->
            <v-card class="pa-8" color="blue-grey lighten-4">
<!--              <v-row>-->
<!--                <v-col cols="10" offset="1">-->
                  <v-text-field
                    label="First Name"
                    v-model="firstName"
                    :error-messages="firstNameErrors"
                    @input="$v.firstName.$touch()"
                    @blur="$v.firstName.$touch()"
                    required
                  ></v-text-field>
<!--                </v-col>-->
<!--              </v-row>-->
<!--              <v-row>-->
<!--                <v-col cols="10" offset="1">-->
                  <v-text-field
                    label="Last Name"
                    v-model="lastName"
                    :error-messages="lastNameErrors"
                    @input="$v.lastName.$touch()"
                    @blur="$v.lastName.$touch()"
                    required
                  ></v-text-field>
<!--                </v-col>-->
<!--              </v-row>-->
<!--              <v-row>-->
<!--                <v-col cols="10" offset="1">-->
                  <v-text-field
                    label="Email"
                    v-model="email"
                    :error-messages="emailErrors"
                    @input="$v.email.$touch()"
                    @blur="$v.email.$touch()"
                    required
                  ></v-text-field>
<!--                </v-col>-->
<!--              </v-row>-->
<!--              <v-row>-->
<!--                <v-col cols="10" offset="1">-->
                  <v-text-field
                    label="Phone Number"
                    v-model="phoneNumber"
                    :error-messages="phoneNumberErrors"
                    @input="$v.phoneNumber.$touch()"
                    @blur="$v.phoneNumber.$touch()"
                    required
                  ></v-text-field>
<!--                </v-col>-->
<!--              </v-row>-->
<!--              <v-row>-->
<!--                <v-col cols="10" offset="1">-->
                  <v-text-field
                    label="Address"
                    v-model="address"
                    :error-messages="addressErrors"
                    @input="$v.address.$touch()"
                    @blur="$v.address.$touch()"
                    required
                  ></v-text-field>
<!--                </v-col>-->
<!--              </v-row>-->
              <v-row align="center">
                <v-col cols="6" offset="3">
                  <v-btn @click="saveUser" color="indigo darken-4" class="white--text mx-2">Add user</v-btn>
                  <v-btn @click="cancelAdding" color="blue-grey" class="white--text mx-2">Cancel</v-btn>
                </v-col>
                <v-col>
                </v-col>
              </v-row>
            </v-card>
    <!--      </v-col>-->
    <!--    </v-row>-->
    <!--  </v-container>-->
    </v-dialog>
  </v-row>
</template>

<script>
import db from '../../libs/firebaseInit'
import { required, minLength } from 'vuelidate/lib/validators'
export default {
  name: 'AddUserDialog',
  data: () => ({
    firstName: '',
    lastName: '',
    email: '',
    phoneNumber: '',
    address: ''
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
      // this.$v.$touch()
      // if (!this.$v.$dirty) {
      await db.firestore().collection('users').add({
        first_name: this.firstName,
        last_name: this.lastName,
        email: this.email,
        phone_number: this.phoneNumber,
        address: this.address
      })
      this.$emit('updateAddUserDialogVisibility', false)
      // } else {
      //   this.$v.$reset()
      // }
    },
    cancelAdding () {
      this.$emit('updateAddUserDialogVisibility', false)
    }
  },
  computed: {
    firstNameErrors () {
      const errors = []
      if (!this.$v.firstName.$dirty) return errors
      !this.$v.firstName.required && errors.push('First name is required.')
      return errors
    },
    lastNameErrors () {
      const errors = []
      if (!this.$v.lastName.$dirty) return errors
      !this.$v.lastName.required && errors.push('Last name is required.')
      return errors
    },
    emailErrors () {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.required && errors.push('Email is required.')
      return errors
    },
    phoneNumberErrors () {
      const errors = []
      if (!this.$v.phoneNumber.$dirty) return errors
      !this.$v.phoneNumber.required && errors.push('Phone number is required.')
      !this.$v.phoneNumber.minLength && errors.push('Phone number must have at least 9 characters')
      return errors
    },
    addressErrors () {
      const errors = []
      if (!this.$v.address.$dirty) return errors
      !this.$v.address.required && errors.push('Address is required.')
      return errors
    }
  },
  watch: {
    'addUserDialogVisibility' (value) {
      if (value === false) {
        this.$v.$reset()
        this.firstName = ''
        this.lastName = ''
        this.email = ''
        this.phoneNumber = ''
        this.address = ''
      }
    }
  }
}
</script>

<style>
</style>
