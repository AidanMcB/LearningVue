<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <!-- directive called vbind pass todos from the data, passed in as a prop -->
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from './components/Todos';
import Header from './components/layout/Header'
import AddTodo from './components/AddTodo'
import axios from 'axios';
export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res =>  {this.todos = this.todos.filter(todo => todo.id !== id);
      return res})
      .catch(err => console.log(err))
     
    },
    addTodo(newTodo) {
      // json placeholder gives you an id
      const { title, completed } = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos/', {
        title,
        completed
      })
      .then(res => this.todos = [...this.todos, res.data])
      .catch( err => console.log(err))
      
    }
  },
  // runs right away, similar to componentDidMount()
  created() {
     axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
     .then(res => this.todos = res.data)
     .catch(err => console.log(err))
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
</style>
