<template>
<v-container fluid>
  <v-row>
    <v-col cols="6" offset="3">
      <v-card height="500" color="#9FA8DA"></v-card>
    </v-col>
  </v-row>
</v-container>
</template>

<script>
// import db from '../libs/firebaseInit'
export default {
  name: 'Dashboard',
  data () {
    return {
      details: []
    }
  },
  // mounted () {
  //   db.firestore().collection('users').get()
  //     .then(querySnapshot => {
  //       querySnapshot.forEach(doc => {
  //         console.log(doc.data())
  //       })
  //     })
  // },
  watch: {
    'details' (value) {
      console.log(value)
    }
  }
}
</script>
