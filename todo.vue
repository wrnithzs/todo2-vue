<template>
    <div id="app" class="container">
        <div class="page-header">
            <h1>Vue.js & Firebase <small>Todo Application</small></h1>
        </div>
        <div class="card">
            <div class="card-body">
                <h3 class="card-title">Add New Todo</h3>
                <label for="bookTitle">Title:</label>
                
                <input type="text" v-model="newTask" class="form-control">
                <br>
                <button type="button" class="btn btn-info" v-on:click="addTodo()">
                    Add Todo
                </button> 
            </div>
        </div>
        <br>
        <div class="card">
            <div class="card-body">
                <h3 class="card-title">Todo List</h3>
                <button type="button" class="btn btn-warning" v-on:click="loadTodo()"> Load Todo </button><br><br>
                <table class="table">
                    <thead class="thead-dark">
                        <tr>
                            <th scope="col">task</th>
                            <th scope="col">Time</th>
                            <th scope="col">Done</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(todo) in todos" v-bind:key="todo.id"> 
                            <td>{{todo.task}}</td>
                            <td>{{todo.timestamp}}</td>
                            <td>
                                <button class="btn btn-danger" v-on:click="removeTodo(todo.id)"> Delete </button>
                            </td>
                        </tr>
                    </tbody>
                </table> 
        </div>
    </div>
  </div>
</template>

<!--firebase config-->
<script src="https://www.gstatic.com/firebasejs/7.7.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/7.7.0/firebase-firestore.js"></script>

<script>
    import { firebase } from "@firebase/app";
    import "@firebase/firestore";
       
    var firebaseConfig = {
        apiKey: "AIzaSyBGXFuW2-dgcqzr2Qi3FJHHUNgqchi6qNw",
        authDomain: "mytodo-9c7de.firebaseapp.com",
        databaseURL: "https://mytodo-9c7de.firebaseio.com",
        projectId: "mytodo-9c7de",
        storageBucket: "mytodo-9c7de.appspot.com",
        messagingSenderId: "971316618674",
        appId: "1:971316618674:web:d3644e81ae09d909f630ab"
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore(); 

    export default{
        data() {
            return {
                todos:[],
                newTask:'',
                removeTask:'' 
            }
        },
        firestore() {
            return {
                todos:db.collection('todos'),
            }
        },
        methods:{
            addTodo() {
                db.collection('todos').add({
                    task : this.newTask,
                    timestamp : new Date(),
                });
                this.loadTodo(); 
            },
            loadTodo() {
                let todolist = [];
                db.collection('todos').get().then(function(querySnapshot) {
                    querySnapshot.forEach(function(doc) {
                    let todo = {
                        id: doc.id,
                        task: doc.data().task,
                        timestamp: doc.data().timestamp.toDate()
                    }
                    todolist.push(todo)
                    });
                });
                this.todos = todolist;
            },
            removeTodo(collectionID) {
                db.collection('todos').doc(collectionID).delete().then(function() {
                    }).catch(function(error) {
                        console.error("Error removing document: ", error);
                        });  
                this.loadTodo();        
                    }         
        }
    }
</script>

<style>
    @import url("https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css");
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}
</style>
