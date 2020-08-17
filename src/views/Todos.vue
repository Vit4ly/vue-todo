<template>
  <div>
    <h2>Todo Application</h2>
    <router-link to="/">Home</router-link>
    <hr>
    <AddTodo
        @add-todo="addTodo"
    />

    <select v-model="filter">
      <option value="all">All</option>
      <option value="complited">Complited</option>
      <option value="not-complited">Not Complited</option>
    </select>

    <hr>

    <Loader v-if="loading"/>
    <TodoList
        v-else-if="todos.length"
        v-bind:todos="filteredTodos"
        @remove-todo="removeTodo"/>
    <p v-else>No Todo</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList.vue';
import AddTodo from "@/components/AddTodo";
import Loader from "@/components/Loader";

export default {
  name: "app",
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
        .then(response => response.json())
        .then(json =>
            setTimeout(() => {
              this.todos = json
              this.loading = false
            }, 1000)
        )
  },
  // watch: {
  //   filter(value) {
  //
  //   }
  // },
  computed: {
    filteredTodos() {
      if (this.filter === 'all') {
        return this.todos
      }
      if (this.filter === 'complited') {
        return this.todos.filter(t => t.complited)
      }
      if (this.filter === 'not-çomplited') {
        return this.todos.filter(t => !t.complited)
      }

    }
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo) {
      this.todos.push(todo)
    }
  },
  components: {
    TodoList, AddTodo, Loader
  }
}
</script>

<style scoped>

</style>
