<template>
  <div id="app">
    <div>
      Author:   <input type="text" v-model="newbook.author" placeholder="auther">
      Title:  <input type="text" v-model="newbook.title" placeholder="title">
      <button @click="addnewbook">ADD</button>
    </div>
    <div v-for="(book,index) in books" v-bind:key="book['.key']">
      <p v-if="!edit"> {{index +1}}:  Author: {{book.author}}  Title: {{book.title}}     <span style="color:blue" @click="edit=true">Edit</span>    <span style="color:red" @click="removebook(book)">Delete</span></p>
      <p v-else>Author:   <input type="text" v-model="book.author">  <span style="color:green" @click="editbook">Save</span>   <span style="color:red" @click="edit = false">Cancel</span></p>
    </div>

  </div>
</template>

<script>
import Firebase from 'firebase'
import toastr from 'toastr'
let config = {
  apiKey: 'AIzaSyCDz5gqnF9aTMogOINtv3Ssr9j5HhG1do8',
  authDomain: 'hello-b49e3.firebaseapp.com',
  databaseURL: 'https://hello-b49e3.firebaseio.com',
  projectId: 'hello-b49e3',
  storageBucket: 'hello-b49e3.appspot.com',
  messagingSenderId: '933286563503'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let bookref = db.ref('books')
export default {
  name: 'App',
  data () {
    return {
      newbook: {
        author: '',
        title: ''
      },
      edit: false
    }
  },
  firebase: {
    books: bookref
  },
  methods: {
    addnewbook () {
      console.log(this.newbook.author)
      console.log(this.newbook.title)
      bookref.push(this.newbook)
      this.newbook.author = ''
      this.newbook.title = ''
    },
    removebook (book) {
      bookref.child(book['.key']).remove()
      toastr.success('Book removed')
    },
    editbook (book) {
      bookref.child(book['.key']).set(this.newbook.author)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
