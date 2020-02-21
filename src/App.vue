<template>
  <div id="app">
    <img src="./assets/logo.png">
    <div>
      <h1>Todo List</h1>
        <p>Tache en cours: {{ countNotChecked }}</p>
        <ul>
          <li v-bind:class="{'checked' :todo.checked}" v-for="todo in todos">{{ todo.value }}<button  v-on:click="deleteTodo(todo)">X</button><input v-model="todo.checked" type="checkbox"></li>
        </ul>
      <input v-model="input" type="text"><button :disabled='isDisabled' v-on:click="addTodo">+</button>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
export default {
  name: 'App',
  data() {
    return {
      todos: [],
      input: undefined,
    }
  },
  methods: {
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
      console.log(this.todos.filter(todo => todo.checked));
    },
    deleteTodo: function (todo) {
      let index = this.todos.indexOf(todo)
      this.todos.splice(index, 1)
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
