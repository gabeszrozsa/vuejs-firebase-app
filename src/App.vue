<template>
  <div id="app" class="container">


    <div class="card">
      <div class="card-body">
        <h4 class="card-title">Vue.js 2 & Firebase app</h4>
        <h6 class="card-subtitle mb-2 text-muted">Add Book</h6>
        <p class="card-text">
          <form id="form" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
            </div>
            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
            </div>
            <div class="form-group">
              <label for="bookUrl">Url:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
            </div>
            <input type="submit" class="btn btn-primary" value="Add Book">
          </form>
        </p>
        <h6 class="card-subtitle mb-2 text-muted">Books list</h6>
        <p class="card-text">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Title</th>
                <th>Author</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="book in books">
                <td>
                  <a v-bind:href="book.url">
                    {{book.title}}
                  </a>
                </td>
                <td>{{book.author}}</td>
                <td>
                  <button type="button" class="btn btn-danger" v-on:click="removeBook(book)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </p>
      </div>
    </div>



  </div>
</template>

<script>
import toastr from 'toastr';

// Firebase config
import Firebase from 'firebase';

let config = {
  apiKey: "AIzaSyC6O3NOabKuNuxwwERTGl4Lqvym4EloQFk",
  authDomain: "vuejs-firebase-app-5f016.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-app-5f016.firebaseio.com",
  projectId: "vuejs-firebase-app-5f016",
  storageBucket: "vuejs-firebase-app-5f016.appspot.com",
  messagingSenderId: "34641924396"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');

export default {
  name: 'app',
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
    };
  },
  methods: {
    addBook: function() {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
    },
    removeBook: function(book) {
      booksRef.child(book['.key']).remove();
      toastr.success('Book removed');
    }
  }
};
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
