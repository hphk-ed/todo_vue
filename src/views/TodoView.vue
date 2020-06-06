<template>
  <div>
    <h1>todotodo</h1>
    <TodoInput @new-todo="createTodo"/>
    <TodoList :todoList="todos" @update="updateTodo" @delete="deleteTodo"/>
  </div>
</template>

<script>
import axios from 'axios'

import TodoInput from "../components/TodoInput.vue";
import TodoList from "../components/TodoList.vue";

const BASE_URL = process.env.VUE_APP_BASE_URL

export default {
  name: "TodoView",
  components: {
    TodoInput,
    TodoList,
  },

  data() {
    return {
      todos: [],
    };
  },

  methods: {

    accessServer(context) {
      axios(context)
      .then(res => {
        // console.log(res)
        this.todos = res.data.todos
      })
      .catch(err => {
        console.error(err)
      })
    },
    
    createTodo(todo) {
      const context = {
        method: 'post',
        url: BASE_URL+todo.id+'/',
        data: todo,
      }
      this.accessServer(context)
    },

    updateTodo(todo) {
      const context = {
        method: 'put',
        url: BASE_URL+todo.id+'/',
        data: todo,
      }
      this.accessServer(context)
    }, 

    deleteTodo(todo){
      const context = {
        method: 'delete',
        url: BASE_URL+todo.id+'/',
        data: todo,
      }
      this.accessServer(context)
    }

  },

  created() {
    const context = {
        method: 'get',
        url: BASE_URL,
    }
    this.accessServer(context)
  }
};
</script>

<style></style>
