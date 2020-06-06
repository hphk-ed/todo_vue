<template>
  <div>
    <h1>Todo Todo Ddu</h1>
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

    connectServer(method, url, data={}){
      const context = {
        method, url, data
      }

      axios(context)
      .then(res => {
        // console.log(res)
        this.todos = res.data.todos
      })
      .catch(err => {
        console.error(err)
      })
    },
    
    createTodo(input) {
      const URL = BASE_URL+'todo/'
      this.connectServer('post', URL, input)
    },

    updateTodo(todo) {
      const URL = BASE_URL+'todo/'+todo.id+'/'
      this.connectServer('put', URL, todo)
    }, 

    deleteTodo(todo){
      const URL = BASE_URL+'todo/'+todo.id+'/'
      this.connectServer('delete', URL, todo)
    }

  },

  created() {
    this.connectServer('get', BASE_URL)
  }
};
</script>

<style></style>
