<template>
  <v-container>
    <v-row>
      <v-col cols="8" offset="2">
        <v-card color="indigo lighten-3" class="pa-2">
          <div v-if="users.length === 0" align="center">
            <v-progress-circular
              indeterminate
              color="primary"
            ></v-progress-circular>
          </div>
          <div v-else class="pa-1">
            <v-row align="center">
              <v-col cols="10">
                <v-card color="indigo darken-1" class="white--text font-weight-bold pa-1">
                  <v-row>
                    <v-col cols="3" style="text-align: center">First Name</v-col>
                    <v-col cols="3" style="text-align: center">Last Name</v-col>
                    <v-col cols="6" style="text-align: center">Email</v-col>
                  </v-row>
                </v-card>
              </v-col>
              <v-col cols="2">
                <v-btn @click="addUserDialogVisibility = true" color="green" class="white--text">Add user</v-btn>
              </v-col>
            </v-row>
            <v-row align="center">
              <v-col cols="12">
                <div v-for="user in users" :key="user.id">
                  <v-row>
                    <v-col cols="10">
                      <v-card>
                        <v-row>
                          <v-col cols="3" style="text-align: center">{{user.firstName}}</v-col>
                          <v-col cols="3" style="text-align: center">{{user.lastName}}</v-col>
                          <v-col cols="6" style="text-align: center">{{user.email}}</v-col>
                        </v-row>
                      </v-card>
                    </v-col>
                    <v-col cols="1">
                      <v-tooltip bottom>
                        <template v-slot:activator="{ on }">
                          <v-btn fab small color="indigo lighten-1" v-on="on" @click="showUserDetails(user.id), detailsDialogVisibility = true">
                            <v-icon style="color: white">mdi-account-card-details</v-icon>
                          </v-btn>
                        </template>
                        <span>Karta użytkownika</span>
                      </v-tooltip>
                    </v-col>
                    <v-col cols="1">
                      <v-tooltip bottom>
                        <template v-slot:activator="{ on }">
                          <v-btn @click="deleteUser(user.id)" fab small color="red" v-on="on">
                            <v-icon style="color: white">mdi-delete</v-icon>
                          </v-btn>
                        </template>
                        <span>Usuń użytkownika</span>
                      </v-tooltip>
                    </v-col>
                  </v-row>
                </div>
              </v-col>
<!--              <v-col cols="2">-->
<!--                <v-row>-->
<!--                  <v-col cols="6" style="text-align: center">-->
<!--                  </v-col>-->
<!--                  <v-col cols="6" style="text-align: center">-->
<!--                  </v-col>-->
<!--                </v-row>-->
<!--              </v-col>-->
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
    <AddUserDialog
      :addUserDialogVisibility.sync="addUserDialogVisibility"
      v-on:updateAddUserDialogVisibility="updateAddUserDialogVisibility"
    />
  </v-container>
</template>

<script>
import db from '../libs/firebaseInit'
import DetailsDialog from '../components/ui/DetailsDialog'
import AddUserDialog from '../components/ui/AddUserDialog'
export default {
  name: 'Employees',
  components: { AddUserDialog, DetailsDialog },
  data: () => ({
    users: [],
    userDetails: {},
    detailsDialogVisibility: false,
    addUserDialogVisibility: false,
    circular: false
  }),
  methods: {
    updateDetailsDialogVisibility (value) {
      this.detailsDialogVisibility = value
    },
    showUserDetails (id) {
      this.users.forEach(user => {
        if (user.id === id) {
          this.userDetails = {
            id: user.id,
            firstName: user.firstName,
            lastName: user.lastName,
            phoneNumber: user.phoneNumber,
            email: user.email,
            address: user.address
          }
        }
      })
    },
    updateAddUserDialogVisibility (value) {
      this.users = []
      this.addUserDialogVisibility = value
      this.getUsers()
    },
    async getUsers () {
      let userRef = db.firestore().ref('/project/vue-firebase-crud-7846d/database/firestore/data~2Fusers')
      console.log(userRef)
    },
    deleteUser (id) {
      db.firestore().collection('users').get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            if (doc.id === id) {
              doc.ref.delete()
            }
          })
        })
      this.getUsers()
    }
  },
  created () {
    this.getUsers()
  },
  watch: {
    'addEmployeeDialogVisibility' (value) {
      if (value === false) {
        this.getUsers()
      }
    },
    'users' (value) {
      console.log(value)
    }
  }
}
</script>
<style>
  .fill-height {
    height: 40%;
  }
</style>
