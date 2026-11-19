<script setup>
import { ref, computed, watch } from 'vue'
import AppHeader from './components/Header.vue'
import TodoItem from './components/Todoitem.vue'

const newTask = ref('')
const newPriority = ref(' (Middel)')
const filter = ref('Alles')

// load from Local Storage on start
const todos = ref(JSON.parse(localStorage.getItem('todos') || '[]'))

// save whenever the list changes
watch(
  todos,
  (value) => localStorage.setItem('todos', JSON.stringify(value)),
  { deep: true }
)

function addTodo() {
  const text = newTask.value.trim()
  if (!text) return
  todos.value.push({
    id: Date.now(),
    text,
    done: false,
    priority: newPriority.value,
  })
  newTask.value = ''
}

function removeTodo(id) {
  todos.value = todos.value.filter((t) => t.id !== id)
}

function toggleTodo(id) {
  const todo = todos.value.find((t) => t.id === id)
  if (todo) todo.done = !todo.done
}

// number of open tasks
const openCount = computed(() => todos.value.filter((t) => !t.done).length)

// filtered list
const filteredTodos = computed(() => {
  if (filter.value === 'Open') return todos.value.filter((t) => !t.done)
  if (filter.value === 'Afgerond') return todos.value.filter((t) => t.done)
  return todos.value
})
</script>

<template>
  <div class="min-h-screen bg-slate-100 p-6">
    <div class="mx-auto max-w-xl">
      <AppHeader :open-count="openCount" />

      <!-- add tasks -->
      <form @submit.prevent="addTodo" class="mb-4 flex gap-2">
        <input
          v-model="newTask"
          type="text"
          placeholder="Nieuwe taak..."
          class="flex-1 rounded-lg border border-slate-300 bg-white px-3 py-2"
        />
        <!-- priority -->
        <select v-model="newPriority" class="rounded-lg border border-slate-300 bg-white px-2">
          <option> (Hoog)</option>
          <option> (Middel)</option>
          <option> (Laag)</option>
        </select>
        <button
          type="submit"
          @click="addTodo"
          class="rounded-lg bg-blue-600 px-4 py-2 font-medium text-white hover:bg-blue-700"
        >
          Toevoegen
        </button>
      </form>

      <!-- filter buttons -->
      <div class="mb-4 flex gap-2">
        <button
          v-for="f in ['Alles', 'Open', 'Afgerond']"
          :key="f"
          @click="filter = f"
          class="rounded-lg px-3 py-1 text-sm"
          :class="filter === f ? 'bg-blue-600 text-white' : 'bg-white text-slate-700'"
        >
          {{ f }}
        </button>
      </div>

      <!-- conditional rendering -->
      <p v-if="filteredTodos.length === 0" class="text-center text-slate-500">
        Nog geen taken toegevoegd
      </p>

      <!-- v-for -->
      <ul v-else class="space-y-2">
        <TodoItem
          v-for="todo in filteredTodos"
          :key="todo.id"
          :todo="todo"
          @toggle="toggleTodo"
          @remove="removeTodo"
        />
      </ul>
    </div>
  </div>
</template>