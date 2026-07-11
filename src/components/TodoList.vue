<template>
  <ul class="todo-list" v-for="todo in todoList" :key="todo.id">
    <li class="todo-item" v-if="todo.text && todo.text.trim() !== ''">
      <div class="edit-mode" v-if="editingId === todo.id">
        <input
          type="text"
          v-model="editText"
          @keyup.enter="saveEdit(todo)"
          ref="editInput"
        />
        <button @click="saveEdit(todo)" class="btn-success">Save</button>
        <button @click="cancelEdit" class="btn-delete">Cancel</button>
      </div>
      <div v-else class="view-mode">
        {{ todo.text }}
        <div class="todo-actions">
          <button @click="deleteTodo(todo)" class="btn-delete">Delete</button>
          <button @click="startEdit(todo)" class="btn-edit">Edit</button>
        </div>
      </div>
    </li>
  </ul>
</template>
<script setup lang="ts">
import { nextTick, ref } from 'vue'
import type {Todo} from '@/types/todo.ts'

interface Props {
  todoList: Todo[]
}
defineProps<Props>()

const emit = defineEmits<{
  (e: 'delete-todo', todo:Todo): void
  (e: 'save-edit', todo:Todo): void
}>()

const editText = ref<string>('')
const editingId = ref<number | null>(null)
const editInput = ref<HTMLInputElement | null>(null)

function deleteTodo(todo: Todo): void {
  emit('delete-todo', todo)
}
function startEdit(todo: Todo): void {
  editText.value = todo.text
  editingId.value = todo.id
  nextTick(() => {
    if(editInput.value) {
      editInput.value.focus()
    }
  })
}
function saveEdit(todo: Todo): void {
  const trimmedText: string = editText.value.trim()
  if (trimmedText === '') return
  const updatedTodo: Todo = {
    ...todo,
    text: trimmedText
  }
  emit('save-edit', updatedTodo)
  editingId.value = null
}
function cancelEdit(): void {
  editingId.value = null
}
</script>

<style scoped>
.todo-list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 18px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
  transition: transform 0.1s, box-shadow 0.2s;
}
.todo-item:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  transform: translateY(-1px);
}
.todo-actions {
  display: flex;
  margin-top: 10px;
  gap: 6px;
}
.btn-edit,
.btn-delete {
  padding: 6px 20px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s;
  background: transparent;
  line-height: 1;
}
.btn-edit:hover {
  background: #ebf5ff;
  transform: scale(1.1);
}
.btn-delete:hover {
  background: #fff5f5;
  transform: scale(1.1);
}
.edit-mode {
  display: flex;
  gap: 8px;
  flex: 1;
}
.edit-mode input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #4a90d9;
  border-radius: 8px;
  font-size: 16px;
  outline: none;
}
.edit-mode button {
  padding: 6px 10px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s;
  background: transparent;
}
.edit-mode button:hover {
  transform: scale(1.1);
}
.view-mode {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}
</style>
