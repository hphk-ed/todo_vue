<template>
  <li class="list-group-item list-group-item-action text-left">
    <div class="d-flex justify-content-between align-items-center">
      <div class="d-flex align-items-center">
        <input type="checkbox" :checked="todo.is_completed" @click="updateChecked">
        <p class="ml-3 my-auto" v-if="!isEditable" :class="{'done': todo.is_completed}">{{ todo.title }}</p>
        <input class="ml-3 my-auto" v-else v-model="inputText" @keydown.enter="updateText"> 
        <span class="ml-3 timefont"> {{ setTime }} </span>
      </div>
      <div>
        <button class="badge badge-success mr-1" @click="switchEdit" v-if="!isEditable">E</button>
        <button class="badge badge-warning mr-1" @click="switchEdit" v-else>C</button>
        <button class="badge badge-danger" @click="deleteTodo">X</button>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: 'TodoListItem',
  data() {
    return {
      isEditable: false,
      inputText: this.todo.title,
    }
  },
  props: {
    todo: Object,
  },

  computed: {
    setTime() {
      const created = new Date(this.todo.updated_at)
      return `${created.getFullYear()}-${created.getMonth()+1}-${created.getDate()} ${created.getHours()}:${created.getMinutes()}`
    }
  },

  methods: {
    updateChecked() {
      const typeData = {
        type: 'check',
      }
      this.$emit('update', this.todo, typeData)
    },

    updateText() {
      const typeData = {
        type: 'text',
        edit: this.inputText,
      }
      this.$emit('update', this.todo, typeData)
      this.switchEdit()
    },

    deleteTodo() {
      this.$emit('delete', this.todo)
    },

    switchEdit() {
      this.isEditable = !this.isEditable
    },
  }
}
</script>

<style scoped>
.timefont {
  color: teal;
  font-size: 10px;
}

.done {
  text-decoration-line: line-through;
  color: lightgray;
}
</style>
