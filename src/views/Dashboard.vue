<template>
<v-container fluid>
  <v-row>
    <v-col cols="8" offset="2">
      <v-card height="830" color="indigo darken-1" class="pa-2">
        <v-row>
          <v-col cols="10" offset="1">
            <v-card flat color="indigo darken-1" height="50">
              <v-card-text style="font-size: 20px" class="white--text font-weight-bold" align="center">POSTS
                OVERVIEW</v-card-text>
            </v-card>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-card id="scroll" height="650" align="center" color="white">
              <div v-for="post in posts" :key="post.id" class="my-8">
                <div>
                  <v-card width="600" class="my-2 pa-2 font-italic white--text" color="amber accent-4">
                    <v-card-text class="white--text font-weight-bold" style="font-size: 20px">{{post.title}}</v-card-text>
                    {{post.body}}
                    <v-card-text class="white--text" align="right">{{post.date}}</v-card-text>
                    <div align="right">
                      <v-btn @click="goToComments(post)" dark icon>
                        <v-icon>mdi-comment</v-icon>
                      </v-btn>
                    </div>
                  </v-card>
                <v-card width="300" height="30" class="my-2 pa-1 font-italic white--text" color="grey darken-1">
                  {{post.author}}
                </v-card>
                </div>
              </div>
            </v-card>
            <div align="center" class="my-6">
            <v-btn @click="postDialogVisibility = true" color="green" class="white--text">Add post</v-btn>
            </div>
          </v-col>
        </v-row>
      </v-card>
    </v-col>
  </v-row>
  <CommentDialog
    :commentDialogVisibility.sync="commentDialogVisibility"
    v-on:closeDialog="closeCommentDialog"
    :postDetails="postDetails"
  />
  <PostDialog
    :postDialogVisibility.sync="postDialogVisibility"
    v-on:closeDialog="closePostDialog"
  />
</v-container>
</template>

<script>
import db from '../libs/firebaseInit'
import CommentDialog from '../components/ui/CommentDialog'
import PostDialog from '../components/ui/PostDialog'
export default {
  name: 'Dashboard',
  components: { PostDialog, CommentDialog },
  data () {
    return {
      details: [],
      posts: null,
      commentDialogVisibility: false,
      postDialogVisibility: false,
      postDetails: null
    }
  },
  methods: {
    async getPosts () {
      let snapshot = await db.firestore().collection('posts').get()
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
    closeCommentDialog (value) {
      this.commentDialogVisibility = value
    },
    closePostDialog (value) {
      this.postDialogVisibility = value
    },
    goToComments (post) {
      this.postDetails = post
      this.commentDialogVisibility = true
    }
  },
  created () {
    this.getPosts()
  }
}
</script>
