<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import AddTodo from '../components/AddTodo';
import Todos from '../components/Todos';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    AddTodo,
    Todos
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo( id ) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/{$id}`)
        .then( () => this.todos = this.todos.filter(todo => todo.id !== id) )
        .catch( error => console.log( error ) );
    },
    addTodo( newTodo ) {
      const { id, title, completed } = newTodo;
      axios.post("https://jsonplaceholder.typicode.com/todos",{
        title, 
        completed,
        id,
      })
        .then( response => {
          this.todos = [ ...this.todos, response.data ] ;
          console.log( this.todos );
          console.log( response.data );
          console.log( newTodo );
        })
        .catch( error => console.log( error ) );
    },
  },
  created() {
    axios.get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then( response => this.todos = response.data )
      .catch( error => console.log( error ) );
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }
  .btn:hover {
    background: #666;
  }
</style>