<script setup lang="ts"></script>

<template>
  <div class="todo-app">
    <h1>My Todo List</h1>

    <div class="add-todo">
      <input v-model="newTodo" placeholder="What needs to be done?" />
      <button @click="addTodo">Add</button>
    </div>

    <div class="filters">
      <button
        v-for="filter in filters"
        :key="filter"
        @click="currentFilter = filter"
        :class="{ active: currentFilter === filter }"
      >
        {{ filter }}
      </button>
    </div>

    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id" :class="{ completed: todo.completed }">
        <input type="checkbox" v-model="todo.completed" />
        <span>{{ todo.text }}</span>
        <button @click="removeTodo(todo.id)" class="delete">x</button>
      </li>
    </ul>

    <p v-if="todos.length === 0" class="empty-state">No todos yet! Add one above.</p>

    <div class="stats" v-if="todos.length > 0">
      <span>{{ remainingCount }} item(s) left</span>
      <button @click="clearCompleted" v-if="completedCount > 0">
        Clear completed ({{ completedCount }})
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([
  { id: 1, text: 'Learn Vue.js basics', completed: false },
  { id: 2, text: 'Build a todo app', completed: false },
  { id: 3, text: 'Master Vue components', completed: false },
])
const currentFilter = ref('All')
const filters = ['All', 'Active', 'Completed']

const filteredTodos = computed(() => {
  if (currentFilter.value === 'active') {
    return todos.value.filter((todo) => !todo.completed)
  }
  if (currentFilter.value === 'Completed') {
    return todos.value.filter((todo) => todo.completed)
  }
  return todos.value
})

const remainingCount = computed(() => {
  return todos.value.filter((todo) => !todo.completed).length
})

const completedCount = computed(() => {
  return todos.value.filter((todo) => todo.completed).length
})

function addTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      completed: false,
    })
    newTodo.value = ''
  }
}
function removeTodo(id) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}

function clearCompleted() {
  todos.value = todos.value.filter((todo) => !todo.completed)
}
</script>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: blue;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-todo input {
  flex: 1;
  padding: 10px;
  border: 2px solid #ddd;
  border-radius: 5px;
  font-size: 15px;
}

.add-todo button {
  padding: 10px 20px;
  background: #4254b9ff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 15px;
}

.add-todo button:hover {
  background: #359268;
}

.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  justify-content: center;
}

.filters button {
  padding: 5px 15px;
  background: #f0f0f0;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  text-transform: capitalize;
}

.filters-button.active {
  background: #4254b9ff;
  color: white;
}

.todo-list {
  list-style: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #eee;
  gap: 10px;
}

.todo-list li.completed span {
  text-decoration: line-through;
  color: #999;
}

.todo-list input[type='checkbox'] {
  width: 15px;
  height: 15px;
  cursor: pointer;
}

.todo-list span {
  flex: 1;
}

.delete {
  background: #ff6b6b;
  color: white;
  border: none;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 15px;
  line-height: 1;
}

.delete:hover {
  background: #ff5252;
}

.empty-state {
  text-align: center;
  color: #999;
  padding: 50px;
}

.stats {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  padding: 10px;
  background: #f9f9f9;
  border-radius: 5px;
}

.stats button {
  padding: 5px 15px;
  background: #ff6b6b;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 15px;
}
</style>
