<template>
  <div class="container">
    <input class="w-75" v-model='input' @keypress.enter="newTodo">
    <button @click="newTodo">추가</button>
  </div>
</template>

<script>
export default {
  name: 'TodoInput',
  data() {
    return {
      input: '',
    }
  },
  methods: {
    newTodo() {
      const inputTodo = {
        id: Date.now(),
        title: this.input,
        isCompleted: false,
      }

      this.$emit('new-todo', inputTodo)
      this.input = ''
    }
  }
}
</script>

<style>

</style>
