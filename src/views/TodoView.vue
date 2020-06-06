<template>
  <div>
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

    methodGetServer(URL, todo='') {
      axios.get(URL, {
        params: todo,
      })
      .then(res => {
        // console.log(res)
        this.todos = res.data.todos
      })
      .catch(err => {
        console.log(URL)
        console.error(err)
      })
    },

    createTodo(todo) {
      const addtodo_URL = BASE_URL+ 'add/'
      this.methodGetServer(addtodo_URL, todo)
    },

    updateTodo(todo) {
      const update_URL = BASE_URL+'update/'+todo.id+'/'
      this.methodGetServer(update_URL, todo)
    }, 

    deleteTodo(todo){
      const delete_URL = BASE_URL+'delete/'+todo.id+'/'
      this.methodGetServer(delete_URL, todo)
    }

  },

  created() {
    this.methodGetServer(BASE_URL)
  }
};
</script>

<style></style>
