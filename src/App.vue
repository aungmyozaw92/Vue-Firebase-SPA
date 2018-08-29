<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue Test With Firebase Simple App</h1>
    </div>
    
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book Form</h3>
      </div>
      <div class="panel-body">
        <form id="form" v-on:submit.prevent="addBook">
          <div class="form-group">
            <label for="exampleInputEmail1">Title</label>
            <input type="text" class="form-control" id="exampleInputEmail1" v-model="newBook.title" placeholder="Enter Title">
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Author</label>
            <input type="text" class="form-control" id="exampleInputPassword1" v-model="newBook.author" placeholder="Author">
          </div>
          <div class="form-check">
          <label for="exampleInputPassword1">Author</label>
            <input type="text" class="form-control" id="exampleInputPassword1" v-model="newBook.url" placeholder="URL">
          </div>
          <button type="submit" class="btn btn-primary" value="Add Book">Submit</button>
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>Author</th> 
              <th>Action</th>             
            </tr>
          </thead>
          <tbody>
            <tr v-for="book in books">
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td><button type="button" class="btn btn-danger btn-sm" v-on:click="removeBook(book)">Remove</button></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
   apiKey: "AIzaSyDBqOA2t_Eh1EQAiB8yw5_e8nEi9aVUWkM",
    authDomain: "vuetest-239cb.firebaseapp.com",
    databaseURL: "https://vuetest-239cb.firebaseio.com",
    projectId: "vuetest-239cb",
    storageBucket: "vuetest-239cb.appspot.com",
    messagingSenderId: "959063962053"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');

export default {
  name: 'App',
  firebase: {
    books: booksRef
  },
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addBook: function () {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book) {
      booksRef.child(book['.key']).remove();
      toastr.success("Book Removed");
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
