<template>
  <div class="columns">
    <div class="column is-two-thirds">
      <div class="box">
        <form @submit.prevent="onSubmit">
          <div class="field">
            <label class="label">Add Your TODO</label>
            <div class="control">
              <input class="input" type="text" placeholder="Enter Your task" v-model="title" />
            </div>
          </div>
        </form>
      </div>
      <div v-for="todo in todos" :key="todo.id">
        <div v-if="!todo.completed" class="box">
          <p>Created at: {{ todo.created_at }}</p>
          <div class="todo_box" @click="toggleComplete(todo)">
            <span>{{ todo.title }}</span>
            <button class="button is-danger is-small is-pulled-right" v-if="todo.completed" @click="deleteTodo(todo.id)">Delete</button>
          </div>
        </div>
        <div v-else class="completed-tasks">
          <div class="box completed-box">
            <p>Created at: {{ todo.created_at }}</p>
            <div class="todo_box completed" @click="toggleComplete(todo)">
              <span>{{ todo.title }}</span>
              <button class="button is-danger is-small is-pulled-right" v-if="todo.completed" @click="deleteTodo(todo.id)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="column">
      <!-- Right side content can go here -->
    </div>
  </div>
</template>

<script>
import shortid from 'shortid'

export default {
  name: "Todos",
  props: ["todos"],
  data() {
    return {
      title: "",
    };
  },
  methods: {
    onSubmit() {
      const new_todo = {
        title: this.title,
        completed: false,
        id: shortid.generate(),
        created_at: new Date().toLocaleString()
      };
      this.$emit("addTodo", new_todo);
      this.title = "";
    },
    toggleComplete(todo) {
      todo.completed = !todo.completed;
      this.$emit('updateTodo', todo);
    },
    deleteTodo(id) {
      this.$emit('deleteTodo', id);
    }
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: #b02e2e;
}

.todo_box {
  transition: all 0.2s;
  cursor: pointer;
}

.todo_box:hover {
  background-color: #f0f3bd;
}

.completed-box {
  background-color: #e6e6e6;
}

.completed-tasks {
  margin-top: 5px;
}

.box {
  margin-bottom: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
}
</style>
