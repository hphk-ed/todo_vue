<template>
  <div class="container">
    <hr>
    <div class="text-left ml-3 py-3">
      <select v-model="showList">
        <option value="all">전체 보기</option>
        <option value="doing">남은 할 일</option>
        <option value="done">완료된 일</option>
      </select>
    </div>
    <ul class="list-group list-group-flush" v-if="todoList">
      <TodoListItem v-for="todo in eachList" :key="todo.id" :todo="todo" @update="updateTodo" @delete="deleteTodo" />
    </ul>
  </div>
</template>

<script>

import TodoListItem from "./TodoListItem.vue";

export default {
  name: "TodoList",
  components: {
    TodoListItem,
  },
  data() {
    return {
      showList: 'all',
    }
  },
  props: {
    todoList: Array,
  },
  methods: {
    updateTodo(todo, typeData){
      if (typeData.type === 'check'){
        todo.is_completed = !todo.is_completed
      }
      else if (typeData.type === 'text'){
        todo.title = typeData.edit
      }
      this.$emit('update', todo)
    },

    deleteTodo(todo) {
      this.$emit('delete', todo)
    }

  }, 
  computed: {
    eachList() {
      return this.todoList.filter(todo => {
        if (this.showList === "doing"){
          return !todo.is_completed
        }
        else if (this.showList === "done"){
          return todo.is_completed
        }
        else {
          return todo
        }
      })
    },
  }
};
</script>

<style></style>
