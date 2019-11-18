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
                  <div align="left">
                    <v-col>
                      <v-btn disabled v-if="editing" @click="edit" color="grey darken-2" class="white--text mx-2">Edit</v-btn>
                      <v-btn v-else @click="edit" color="grey darken-2" class="white--text mx-2">Edit</v-btn>
                      <v-btn @click="closeDialog" color="amber accent-3" class="white--text mx-2">Return to list</v-btn>
                    </v-col>
                  </div>
<!--                  <v-spacer></v-spacer>-->
                  <v-col v-if="editing">
                    <div align="right">
                      <v-btn @click="back" color="red" class="white--text mx-2">Back</v-btn>
                      <v-btn @click="updateData" color="green" class="white--text mx-2">Save</v-btn>
                    </div>
                  </v-col>
                </v-row>
              </v-col>
              <v-col cols="7">
                <v-card color="blue-grey lighten-4" class="pa-2">
                  <v-card color="indigo darken-1" class="white--text pa-3 font-weight-bold" align="center">
                    POSTS
                  </v-card>
                  <v-row align="center">
                    <v-col cols="8" offset="2">
                      <v-card align="center" color="green" class="white--text font-weight-bold">
                        CREATE NEW POST
                        <v-btn @click="createPost = !createPost" dark icon>
                          <v-icon v-if="createPost">mdi-chevron-up</v-icon>
                          <v-icon v-else>mdi-chevron-down</v-icon>
                        </v-btn>
                      </v-card>
                    </v-col>
                  </v-row>
                  <transition name="create_post_popup">
                    <v-row v-if="createPost" align="center">
                      <v-col cols="8" offset="2">
                        <v-card align="center" color="grey" class="white--text font-weight-bold pa-5">
                          <v-text-field
                            height="10px"
                            label="Title"
                            v-model="postTitle"
                            solo
                            color="teal"
                          >
                          </v-text-field>
                          <v-textarea
                            height="123px"
                            label="What do you want to share with others?"
                            v-model="postBody"
                            solo
                            color="teal"
                          >
                          </v-textarea>
                          <div align="right">
                            <v-btn align="right" @click="addPost" color="indigo darken-1" class="white--text">Add
                              post</v-btn>
                          </div>
                        </v-card>
                      </v-col>
                    </v-row>
                  </transition>
                  <v-row>
                    <v-col cols="8" offset="2">
                      <v-card height="35px" align="center" color="grey" class="white--text font-weight-bold pa-1">YOUR
                        POSTS</v-card>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="10" offset="1">
                      <v-card id="scroll" align="center" color="white" class="white--text font-weight-bold"
                              height="300px">
                        <div v-for="post in posts" :key="post.id" class="my-8">
<!--                          <v-card width="300" height="30" class="my-2 pa-1 font-italic white&#45;&#45;text" color="amber accent-4">-->
<!--                            {{post.title}}-->
<!--                          </v-card>-->
                          <div>
                          <v-card width="400" class="my-2 pa-2 font-italic white--text" color="amber accent-4">
                            <v-card-text class="white--text font-weight-bold" style="font-size: 20px">{{post
                              .title}}</v-card-text>
                            {{post.body}}
                          <v-card-text class="white--text" align="right">{{post.date}}</v-card-text>
                          </v-card>
                            <v-btn @click="deletePost(post.id)" color="amber accent-4" fab x-small>
                              <v-icon style="color: white">mdi-delete</v-icon>
                            </v-btn>
                          </div>
                        </div>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-card>
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
    userDetailsHolder: null,
    createPost: false,
    posts: null,
    postTitle: null,
    postBody: null
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
    async updateData () {
      await db.firestore().collection('users').get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            if (doc.id === this.userDetails.id) {
              doc.ref.set(this.userDetails)
            }
          })
        })
      this.editing = false
    },
    async getPosts () {
      let snapshot = await db.firestore().collection('posts').where('userId', '==', this.userDetails.id).get()
      this.posts = snapshot.docs.map((doc) => {
        return {
          ...doc.data(),
          id: doc.id
        }
      })
      this.posts.sort(function (a, b) {
        // Turn your strings into dates, and then subtract them
        // to get a value that is either negative, positive, or zero.
        return new Date(b.date) - new Date(a.date)
      })
      this.posts.reverse()
    },
    async addPost () {
      await db.firestore().collection('posts').add({
        author: this.userDetails.firstName + ' ' + this.userDetails.lastName,
        userId: this.userDetails.id,
        date: this.getDate(),
        title: this.postTitle,
        body: this.postBody
      })
      this.createPost = false
      this.postTitle = null
      this.postBody = null
      this.getPosts()
    },
    getDate () {
      let d = new Date()
      let month = '' + (d.getMonth() + 1)
      let day = '' + d.getDate()
      let year = d.getFullYear()
      let hour = d.getHours() + ':' + d.getMinutes()

      if (month.length < 2) {
        month = '0' + month
      }
      if (day.length < 2) {
        day = '0' + day
      }

      return [year, month, day, hour].join('-')
    },
    async deletePost (id) {
      await db.firestore().collection('posts').get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            if (doc.id === id) {
              doc.ref.delete()
            }
          })
        })
      this.getPosts()
    }
  },
  watch: {
    'detailsDialogVisibility' (value) {
      if (value === true) {
        this.getPosts()
      }
    }
  }
}
</script>

<style>
  .create_post_popup-enter,
  .create_post_popup-leave-to{
    transform: rotateY(50deg);
  }
  .create_post_popup-enter-to,
  .create_post_popup-leave {
    transform: rotateY(0deg);
  }
  .create_post_popup-enter-active,
  .create_post_popup-leave-active {
    transition: transform 400ms;
  }
.container {
  max-width: 100%;
}
  #scroll {
    overflow-y: auto;
  }
</style>
