<template>
  <section class="input-section">
    <input
      type="text"
      class="input-field"
      v-model="inputTodo"
      placeholder="Text"
      @keyup.enter="handleAdd"
    />
    <button @click="handleAdd" class="btn-add">Add</button>
  </section>
</template>
<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits<{
  (e: 'add-todo', text: string): void
}>()

const inputTodo = ref<string>('')

function handleAdd(): void {
  const trimmedTodo: string = inputTodo.value.trim()
  if (trimmedTodo === '') return

  emit('add-todo', trimmedTodo)
  inputTodo.value = ''
}
</script>

<style scoped>
.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 24px;
}
.input-field {
  flex: 1;
  padding: 12px 16px;
  border: 2px solid #e9ecef;
  border-radius: 10px;
  font-size: 16px;
  transition: border-color 0.2s;
  outline: none;
}
.input-field:focus {
  border-color: #4a90d9;
}
.btn-add {
  padding: 12px 24px;
  background: #4a90d9;
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
  white-space: nowrap;
}
.btn-add:hover {
  background: #357abd;
}
</style>
