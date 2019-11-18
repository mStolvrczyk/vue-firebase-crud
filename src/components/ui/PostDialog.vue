<template>
  <v-row justify="center">
    <v-dialog v-model="postDialogVisibility" max-width="800px">
      <v-card class="pa-5" color="grey">
<!--        <div align="center">-->
          <v-row>
            <v-col cols="10" offset="1">
              <v-text-field
                label="Title"
                v-model="postTitle"
                solo
              >
              </v-text-field>
              <v-textarea
                label="What do you want to share with others?"
                v-model="postBody"
                solo
              >
              </v-textarea>
              <v-row>
                <v-col cols="5" offset="7">
                  <v-autocomplete
                    background-color="white"
                    v-model="author"
                    :items="users"
                    flat
                    search="searchValue"
                    hide-no-data
                    clearable
                    item-value="id"
                    item-text="author"
                    label="Pick user"
                    solo
                    return-object
                  >
                    <template v-slot:no-data>
                      <v-list-tile>
                        <v-list-tile-title>
                          Brak stacji
                        </v-list-tile-title>
                      </v-list-tile>
                    </template>
                  </v-autocomplete>
                </v-col>
              </v-row>
              <div align="right">
                <v-btn @click="closeDialog" color="blue-grey" class="white--text mx-2">Cancel</v-btn>
                <v-btn color="indigo darken-1" class="white--text">Add
                  post</v-btn>
              </div>
            </v-col>
          </v-row>
<!--        </div>-->
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import db from '../../libs/firebaseInit'
export default {
  name: 'PostDialog',
  data: () => ({
    users: [],
    author: null,
    postTitle: null,
    postBody: null
  }),
  props: {
    postDialogVisibility: Boolean
  },
  methods: {
    closeDialog () {
      this.$emit('closeDialog', false)
    },
    async mapAuthors () {
      db.firestore().collection('users').get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            const data = doc.data().firstName + ' ' + doc.data().lastName
            this.users.push(data)
          })
        })
    }
  },
  watch: {
    'postDialogVisibility' (value) {
      if (value === true) {
        this.mapAuthors()
      }
    },
    'users' (value) {
      console.log(value)
    }
  }
}
</script>

<style scoped>

</style>
