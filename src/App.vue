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

<script setup lang="ts">
import { ref } from 'vue'
import TodoList from './components/TodoList.vue'
import TodoInput from './components/TodoInput.vue'
import type {Todo} from '@/types/todo.ts'

const todoList = ref<Todo[]>([])

function addTodo(text: string): void {
  const newTodo: Todo = {
    text: text,
    id: Date.now(),
  }
  todoList.value.push(newTodo)
}

function deleteTodo(todo: Todo): void {
  const index: number = todoList.value.indexOf(todo)
  if (index > -1) {
    todoList.value.splice(index, 1)
  }
}
function saveEdit(updatedTodo: Todo): void {
  const index: number = todoList.value.findIndex(
    (t: Todo) => t.id === updatedTodo.id)
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
