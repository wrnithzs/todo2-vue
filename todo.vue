<template>
  <div id="app" class="container">
    <nav class="navbar navbar-expand-md navbar-light bg-light mb-4">
      <a class="navbar-brand" href="#">Navbar</a>
      <button
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
        class="navbar-toggler"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a v-on:click="loadTodo(), showCreate =false, showTodo =true" class="nav-link" href="#">
              Todo List
              <span class="sr-only">(current)</span>
            </a>
          </li>
            <li class="nav-item ">
            <a v-on:click="showCreate = true , showTodo = false" class="nav-link" href="#">
              Create Todo
              <span class="sr-only">(current)</span>
            </a>
          </li>
        </ul>
      </div>
    </nav>
    <br/>
      <div v-if="showTodo">
      <div v-for="(todo, index) in todos" v-bind:key="todo.id" class="card mb-1">
        <div class="card-body">
          <h5 class="card-title">Todo {{index+1}}</h5>
          <p class="card-text">{{todo.task }}</p>
          <p class="card-text">{{todo.timestamp }}</p>
          <div class="row">
            <div class="col-auto-mr-auto">
              <button type="button" class="btn btn-info">Edit</button>
              &nbsp;
              <button @click="removeTodo(todo.id)" type="button" class="btn btn-danger">Delete</button>
            </div>
            <div class="col-auto">&nbsp;
              <button class="btn btn-outline-info">Up</button>
            </div>
          </div>
        
        </div>
      </div>
      </div>
      <div v-if="showCreate">
      <div class="form-group">
        <label for="exampleInputEmail1">Input Todo</label>
        <input type="text"  class="form-control"/>
        <label >Descriptions</label>
        <input type="text"  class="form-control"/>
      </div>
      <button type="button" class="btn btn-warning" v-on:click="loadTodo(),showCreate =false, showTodo =true">Cancel</button>&nbsp;
      <button type="button" class="btn btn-primary" v-on:click="addTodo()">Save</button>
      </div>
    <br/>


  </div>
</template>

<!--firebase config-->
<script src="https://www.gstatic.com/firebasejs/7.8.2/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/7.7.0/firebase-firestore.js"></script>

<script>
import { firebase } from "@firebase/app";
import "@firebase/firestore";

  var firebaseConfig = {
    apiKey: "AIzaSyDgf7t1b116sMNtXgJ0lnlIssVlYZAZvu4",
    authDomain: "vue-todo-1c732.firebaseapp.com",
    databaseURL: "https://vue-todo-1c732.firebaseio.com",
    projectId: "vue-todo-1c732",
    storageBucket: "vue-todo-1c732.appspot.com",
    messagingSenderId: "599727970654",
    appId: "1:599727970654:web:cd2027969f09f4dd6dd7cf"
  };

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

export default {
  data() {
    return {
      todos: [],
      newTask: " ",
      showCreate: false,
      showTodo: true,
    }
  },
  firestore() {
    return {
      todos: db.collection("todos")
    }
  },
  methods: {
    addTodo() {
      db.collection("todos").add({
        task: this.newTask,
        timestamp: new Date()
      });
      this.loadTodo();
    },
    loadTodo() {
      let todolist = [];
      db.collection("todos").get().then(function(querySnapshot) {
          querySnapshot.forEach(function(doc) {
            let todo = {
              id: doc.id,
              task: doc.data().task,
              timestamp: doc.data().timestamp.toDate()
            };
            todolist.push(todo);
          });
        });
      this.todos = todolist;
    },
    removeTodo(collectionID) {
      db.collection("todos")
        .doc(collectionID)
        .delete()
        .then(function() {})
        .catch(function(error) {
          console.error("Error removing document: ", error);
        });
      this.loadTodo();
    },
  },
  mounted() {
    this.loadTodo();
  },

};
</script>

<style>
@import url("https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css");
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
