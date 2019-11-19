<template>
  <v-row justify="center">
    <v-dialog persistent v-model="commentDialogVisibility" max-width="800px">
      <v-card class="pa-5" color="indigo darken-1">
        <v-card id="scroll" align="center" height="400px" class="my-2">
          <div v-for="comment in comments" :key="comment.id" class="pa-2">
            <div>
              <v-card width="400" class="my-2 pa-1 font-italic white--text" color="amber accent-4">
                {{comment.body}}
                <v-card-text class="white--text" align="right">{{comment.date}}</v-card-text>
              </v-card>
              <v-card class="white--text" width="300" color="grey">
                {{comment.author}} ({{comment.authorEmail}})
              </v-card>
            </div>
          </div>
        </v-card>
          <v-textarea
          label="Comment post"
          v-model="commentBody"
          solo
          color="teal"
        >
        </v-textarea>
          <v-row>
            <v-col cols="5" offset="7">
              <v-text-field
              solo
              label="author"
              v-model="author"
              >
              </v-text-field>
              <v-text-field
                solo
                label="email"
                v-model="authorEmail"
              >
              </v-text-field>
            </v-col>
          </v-row>
        <div align="right">
          <v-btn @click="closeDialog" color="red" class="white--text mx-2">Close</v-btn>
          <v-btn @click="addComment()" color="green" class="white--text mx-2">Add comment</v-btn>
        </div>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import db from '../../libs/firebaseInit'
export default {
  name: 'CommentDialog',
  data: () => ({
    commentBody: null,
    comments: null,
    author: null,
    authorEmail: null
  }),
  props: {
    commentDialogVisibility: Boolean,
    postDetails: Object
  },
  methods: {
    closeDialog () {
      this.$emit('closeDialog', false)
    },
    async addComment () {
      await db.firestore().collection('comments').add({
        author: this.author,
        authorEmail: this.authorEmail,
        postId: this.postDetails.id,
        date: this.getDate(),
        body: this.commentBody
      })
      this.commentBody = null
      this.getComments()
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
    async getComments () {
      let snapshot = await db.firestore().collection('comments').where('postId', '==', this.postDetails.id).get()
      this.comments = snapshot.docs.map((doc) => {
        return {
          ...doc.data(),
          id: doc.id
        }
      })
      this.comments.sort(function (a, b) {
        return a.date > b.date
      })
      this.comments.reverse()
    }
  },
  watch: {
    'commentDialogVisibility' (value) {
      if (value === true) {
        this.getComments()
      }
    }
  }
}
</script>

<style>
  #scroll {
    overflow-y: auto;
  }
</style>
