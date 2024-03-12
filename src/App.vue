<template>
  <div id="app">
    <div class="section">
      <div class="container">
        <div class="columns">
          <div class="column">
            <Todos :todos="todos" @addTodo="addTodo" @updateTodo="updateTodo" @deleteTodo="deleteTodo" />
          </div>
          <div class="column is-one-third">
            <div class="completed-tasks-container">
              <h4>Completed Tasks</h4>
              <ul>
                <br>
                <li v-for="todo in completedTodos" :key="todo.id" class="completed-task">{{ todo.title }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "bulma/css/bulma.css";
import Todos from './components/Todos.vue'
import axios from 'axios'; // Import Axios for HTTP requests

export default {
  name: 'App',
  components: {
    Todos
  },
  data() {
    return {
      todos: [],
    };
  },
  computed: {
    completedTodos() {
      return this.todos.filter(todo => todo.completed);
    }
  },
  created() {
    // Retrieve todos from the server when the app starts
    this.fetchTodos();
  },
  methods: {
    async fetchTodos() {
      try {
        const response = await axios.get('http://localhost:3000/todos');
        this.todos = response.data;
      } catch (error) {
        console.error('Error fetching todos:', error);
      }
    },
    async addTodo(todo) {
      try {
        const response = await axios.post('http://localhost:3000/todos', todo);
        this.todos.unshift(response.data);
      } catch (error) {
        console.error('Error adding todo:', error);
      }
    },
    async updateTodo(todo) {
      try {
        await axios.put(`http://localhost:3000/todos/${todo.id}`, todo);
      } catch (error) {
        console.error('Error updating todo:', error);
      }
    },
    async deleteTodo(id) {
      try {
        await axios.delete(`http://localhost:3000/todos/${id}`);
        // Update local todos after deletion
        this.todos = this.todos.filter(todo => todo.id !== id);
      } catch (error) {
        console.error('Error deleting todo:', error);
      }
    }
  }
};
</script>

<style>
#app {
  background-color: rgb(95, 147, 220);
  font-family: "Roboto", sans-serif;
  min-height: 100vh;
  padding: 10px;
}

.completed-tasks-container {
  background-color: #ffffff;
  padding: 10px;
  border-radius: 5px;
  
}

.completed-tasks h4 {
  color: #000000;
  font-size: 12px;
  margin-bottom: 8px;
}

.completed-task {
  color: #151414;
  font-size: 17px;
  margin-bottom: 5px;justify-content: center;
  display: flex;
  
}
</style>
