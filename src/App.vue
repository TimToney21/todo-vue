<template>
  <div class="container">
    <TodoInput @add-todo="addTodo" />
    <TodoList
      :todo-list="todoList"
      @delete-todo="deleteTodo"
      @save-edit="saveEdit"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'

const todoList = ref([])

function addTodo(text) {
  todoList.value.push({
    text: text,
    id: Date.now(),
  })
}

function deleteTodo(todo) {
  const index = todoList.value.indexOf(todo)
  if (index > -1) {
    todoList.value.splice(index, 1)
  }
}
function saveEdit(updatedTodo) {
  const index = todoList.value.findIndex(t => t.id === updatedTodo.id)
  if (index > -1) {
    todoList.value[index] = updatedTodo
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: #f8f9fa;
  border-radius: 16px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}
</style>
