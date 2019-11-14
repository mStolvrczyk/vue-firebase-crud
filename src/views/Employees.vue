<template>
  <v-container fluid>
    <v-row>
      <v-col cols="8" offset="2">
        <v-card color="indigo lighten-3" class="pa-2">
          <div v-if="users.length === 0" align="center">
            <v-progress-circular
              indeterminate
              color="primary"
            ></v-progress-circular>
          </div>
          <div v-else>
            <v-row align="center">
              <v-col cols="8">
                <v-card color="indigo darken-1" class="pa-1 white--text font-weight-bold">
                  <v-row>
                    <v-col cols="3" style="text-align: center">First Name</v-col>
                    <v-col cols="3" style="text-align: center">Last Name</v-col>
                    <v-col cols="6" style="text-align: center">Email</v-col>
                  </v-row>
                </v-card>
              </v-col>
              <v-col cols="3" offset="1">
                <v-btn color="green" class="white--text">Add user</v-btn>
              </v-col>
            </v-row>
            <v-row v-for="user in users" :key="user.id" align="center">
              <v-col cols="8">
                <v-card>
                  <v-row>
                    <v-col cols="3" style="text-align: center">{{user.firstName}}</v-col>
                    <v-col cols="3" style="text-align: center">{{user.lastName}}</v-col>
                    <v-col cols="6" style="text-align: center">{{user.email}}</v-col>
                  </v-row>
                </v-card>
              </v-col>
              <v-col cols="4">
                <v-row>
                  <v-col cols="4" style="text-align: center">
                    <v-tooltip bottom>
                      <template v-slot:activator="{ on }">
                        <v-btn fab small color="indigo lighten-1" v-on="on" @click="showUserDetails(user.id), detailsDialogVisibility = true">
                          <v-icon style="color: white">mdi-account-card-details</v-icon>
                        </v-btn>
                      </template>
                      <span>Karta użytkownika</span>
                    </v-tooltip>
                  </v-col>
                  <v-col cols="4" style="text-align: center">
                    <v-tooltip bottom>
                      <template v-slot:activator="{ on }">
                        <v-btn fab small color="indigo lighten-1" v-on="on">
                          <v-icon style="color: white">mdi-account-edit</v-icon>
                        </v-btn>
                      </template>
                      <span>Edytuj dane użytkownika</span>
                    </v-tooltip>
                  </v-col>
                  <v-col cols="4" style="text-align: center">
                    <v-tooltip bottom>
                      <template v-slot:activator="{ on }">
                        <v-btn fab small color="red" v-on="on">
                          <v-icon style="color: white">mdi-delete</v-icon>
                        </v-btn>
                      </template>
                      <span>Usuń użytkownika</span>
                    </v-tooltip>
                  </v-col>
                </v-row>
              </v-col>
            </v-row>
          </div>
        </v-card>
      </v-col>
    </v-row>
    <DetailsDialog
      :detailsDialogVisibility.sync="detailsDialogVisibility"
      v-on:updateDetailsDialogVisibility="updateDetailsDialogVisibility"
      :userDetails="userDetails"
    />
  </v-container>
</template>

<script>
import db from '../libs/firebaseInit'
import DetailsDialog from '../components/ui/DetailsDialog'
export default {
  name: 'Employees',
  components: { DetailsDialog },
  data: () => ({
    users: [],
    userDetails: {},
    detailsDialogVisibility: false
  }),
  methods: {
    updateDetailsDialogVisibility (value) {
      this.detailsDialogVisibility = value
    },
    showUserDetails (id) {
      db.firestore().collection('users').where('user_id', '==', id).get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.userDetails = {
              id: doc.data().user_id,
              firstName: doc.data().first_name,
              lastName: doc.data().last_name,
              phoneNumber: doc.data().phone_number,
              email: doc.data().email,
              address: doc.data().address
            }
          })
        })
    }
  },
  created () {
    db.firestore().collection('users').get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          const data = {
            'id': doc.data().user_id,
            'firstName': doc.data().first_name,
            'lastName': doc.data().last_name,
            'phoneNumber': doc.data().phone_number,
            'email': doc.data().email
          }
          this.users.push(data)
        })
      })
  },
  watch: {
    'userDetails' (value) {
      console.log(value)
    }
  }
}
</script>
