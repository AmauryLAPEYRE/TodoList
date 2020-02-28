<template>
  <div id="app">
    <img src="./assets/logo.png">
    <div>
      <h2>{{dateTime}}</h2>
        <button v-on:click="deleteAllTodo(todos)">Clear List</button>
        <p>{{ countNotChecked }} Current task(s)</p>
        <ul>
          <li v-bind:class="{'checked' :todo.checked}" v-for="todo in todos">
            <input type="text" v-bind:value="todo.value"><button v-on:click="deleteTodo(todo)">X</button><input v-model="todo.checked" type="checkbox">
          </li>
        </ul>
      <input placeholder="Type your task..." v-model="input" type="text"><button :disabled='isDisabled' v-on:click="addTodo">+</button>
    </div>
  </div>
</template>

<script>

import moment from 'moment'
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  data() {
    return {
      todos: [],
      input: undefined,
      date: '',
    }
  },
  methods: {
     moment: function () {
    return moment();
    },
    addTodo: function () {
      if(this.input) {
        this.todos.push(
        {
          value: this.input,
          checked: false,
        }
      )
      this.input = undefined;
      }
    },
    deleteTodo: function (todo) {
      let index = this.todos.indexOf(todo)
      this.todos.splice(index, 1)
    },
    deleteAllTodo: function () {
       this.todos = []
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem('todo'));
  },
  computed: {
  	isDisabled: function() {
    	return !this.input;
    },
    countNotChecked: function() {
      return this.todos.filter(todo => todo.checked == false).length;
    },
    dateTime: function() {
      return moment().format("dddd, Do"); 
    },
    time: function() {
      return moment().format("h:mm");
    }
  },
  watch : {
    todos: {
      deep: true,
      handler() {
        localStorage.setItem('todo', JSON.stringify(this.todos));
      }
    }
  },
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.checked {
  text-decoration: line-through;
  opacity: 0.6;
}
</style>
