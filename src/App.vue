<template>
  <div id="app" class="container">
    <h1>Vue and Firebase</h1>
    <div class="card">
      <div class="card-header">
        <h3>Add a link</h3>
      </div>
      <div class="card-body">
        <form v-on:submit.prevent="addLink()" class="form-inline">
          <div class="form-group">
            <label for="title">Title</label>
            <input
              class="form-control"
              v-model="newLink.title"
              placeholder="Title"
              type="text"
            id="title">
          </div>
          <div class="form-group">
            <label for="author">Autor</label>
            <input
              class="form-control"
              v-model="newLink.author"
              placeholder="Author"
              type="text"
              id="author">
          </div>
          <div class="form-group">
            <label for="url">Url</label>
            <input
              class="form-control"
              v-model="newLink.url"
              placeholder="Url"
              type="text"
              id="url">
          </div>
          <input type="submit" class="btn btn-success" value="Add a Link">
        </form>
      </div>
    </div>
    <hr>
    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Links list</h3>
      </div>
      <div class="card-body">
        <table class="table table-stripped">
          <thead>
          <tr>
            <th>Title</th>
            <th>Author</th>
            <th>Delete</th>
          </tr>
          </thead>
          <tbody>
            <tr v-for="link in links">
              <td>
                <a v-bind:href="link.url" target="_blank">{{link.title}}</a>
              </td>
              <td>
                {{link.author}}
              </td>
              <td>
                <button class="btn btn-danger" v-on:click="deleteLink(link)">
                  <i class="fa fa-trash" aria-hidden="true"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script>

import Firebase from 'firebase';
import toastr from 'toastr';

let config = {
  apiKey: "AIzaSyCK8QrwWC9MCf5_DaGRbuCm1s3Q-aTL5rk",
  authDomain: "vuefire-8bee8.firebaseapp.com",
  databaseURL: "https://vuefire-8bee8.firebaseio.com",
  projectId: "vuefire-8bee8",
  storageBucket: "",
  messagingSenderId: "173681485554"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let linksRef = db.ref('links');

export default {
  name: 'app',
  firebase: {
    links : linksRef
  },
  methods: {
    addLink: function() {
      linksRef.push(this.newLink);
      this.newLink.title = '';
      this.newLink.author = '';
      this.newLink.url = '';
      toastr.success('Link added');

    },
    deleteLink: function(link) {
      linksRef.child(link['.key']).remove();
      toastr.success('Link removed');
    }
  },
  data() {
    return {
      newLink: {
        title:'',
        author:'',
        url:''
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
  .form-control {
    margin:10px;
  }
</style>
