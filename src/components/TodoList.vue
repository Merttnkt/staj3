<template>
  <div class="todo-list-container">
    <h1>Todo Listesi</h1>
    
    <form @submit.prevent="addTodo" class="add-todo-form">
      <input 
        v-model="newTodoText" 
        type="text" 
        placeholder="Yeni todo ekle..."
        class="todo-input"
        required
      />
      <button type="submit" class="add-btn">Ekle</button>
    </form>

    <div v-if="todos.length === 0" class="empty-state">
      Henüz todo eklenmemiş. Yukarıdan yeni bir todo ekleyebilirsin!
    </div>

    <div v-else class="todos-container">
      <div class="filter-buttons">
        <button 
          @click="filter = 'all'" 
          :class="{ active: filter === 'all' }"
          class="filter-btn"
        >
          Tümü ({{ todos.length }})
        </button>
        <button 
          @click="filter = 'active'" 
          :class="{ active: filter === 'active' }"
          class="filter-btn"
        >
          Aktif ({{ activeTodos.length }})
        </button>
        <button 
          @click="filter = 'completed'" 
          :class="{ active: filter === 'completed' }"
          class="filter-btn"
        >
          Tamamlanan ({{ completedTodos.length }})
        </button>
      </div>

      <div class="todo-items">
        <TodoItem 
          v-for="todo in filteredTodos" 
          :key="todo.id" 
          :todo="todo"
          @toggle="toggleTodo"
          @delete="deleteTodo"
        />
      </div>

      <div v-if="completedTodos.length > 0" class="clear-completed">
        <button @click="clearCompleted" class="clear-btn">
          Tamamlananları Temizle
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import TodoItem from './TodoItem.vue'

interface Todo {
  id: number
  text: string
  completed: boolean
}

const todos = ref<Todo[]>([
  {
    id: 1,
    text: "Deneme mesajı - bu todo'yu işaretle!",
    completed: false
  }
])
const newTodoText = ref('')
const filter = ref<'all' | 'active' | 'completed'>('all')

const activeTodos = computed(() => todos.value.filter(todo => !todo.completed))
const completedTodos = computed(() => todos.value.filter(todo => todo.completed))

const filteredTodos = computed(() => {
  switch (filter.value) {
    case 'active':
      return activeTodos.value
    case 'completed':
      return completedTodos.value
    default:
      return todos.value
  }
})

const addTodo = () => {
  if (newTodoText.value.trim()) {
    const newTodo: Todo = {
      id: Date.now(),
      text: newTodoText.value.trim(),
      completed: false
    }
    todos.value.push(newTodo)
    newTodoText.value = ''
  }
}

const toggleTodo = (id: number) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const deleteTodo = (id: number) => {
  todos.value = todos.value.filter(t => t.id !== id)
}

const clearCompleted = () => {
  todos.value = todos.value.filter(todo => !todo.completed)
}
</script>

<style scoped>
.todo-list-container {
  max-width: 700px;
  margin: 0 auto;
  padding: 30px 20px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  border: 1px solid #e9ecef;
}

h1 {
  text-align: center;
  color: #343a40;
  margin-bottom: 40px;
  font-size: 3rem;
  font-weight: 700;
  letter-spacing: -1px;
  text-shadow: none;
}

.add-todo-form {
  display: flex;
  gap: 16px;
  margin-bottom: 40px;
  background: white;
  padding: 8px;
  border-radius: 20px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
  border: 1px solid #dee2e6;
}

.todo-input {
  flex: 1;
  padding: 16px 20px;
  border: none;
  border-radius: 16px;
  font-size: 16px;
  outline: none;
  background: transparent;
  color: #495057;
  font-weight: 500;
}

.todo-input::placeholder {
  color: #adb5bd;
  font-weight: 400;
}

.todo-input:focus {
  background: #f8f9fa;
}

.add-btn {
  padding: 16px 28px;
  background: #007bff;
  color: white;
  border: none;
  border-radius: 16px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 12px rgba(0, 123, 255, 0.3);
  position: relative;
  overflow: hidden;
}

.add-btn:hover {
  background: #0056b3;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(0, 123, 255, 0.4);
}

.add-btn:active {
  transform: translateY(0);
}


.empty-state {
  text-align: center;
  color: #6c757d;
  font-style: italic;
  padding: 60px 30px;
  background: #f8f9fa;
  border-radius: 20px;
  border: 2px dashed #dee2e6;
  font-size: 18px;
  font-weight: 500;
}

.filter-buttons {
  display: flex;
  gap: 12px;
  margin-bottom: 30px;
  justify-content: center;
  background: #f8f9fa;
  padding: 8px;
  border-radius: 16px;
}

.filter-btn {
  padding: 12px 20px;
  border: none;
  background: transparent;
  border-radius: 12px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  color: #6c757d;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
}

.filter-btn:hover {
  color: #495057;
  background: rgba(255, 255, 255, 0.5);
}

.filter-btn.active {
  background: #007bff;
  color: white;
  box-shadow: 0 4px 12px rgba(0, 123, 255, 0.3);
  transform: translateY(-1px);
}

.todo-items {
  margin-bottom: 30px;
  min-height: 100px;
}

.clear-completed {
  text-align: center;
}

.clear-btn {
  padding: 14px 28px;
  background: #dc3545;
  color: white;
  border: none;
  border-radius: 14px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 12px rgba(220, 53, 69, 0.3);
}

.clear-btn:hover {
  background: #c82333;
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(220, 53, 69, 0.4);
}

.clear-btn:active {
  transform: translateY(0);
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.todo-list-container {
  animation: fadeInUp 0.6s ease-out;
}

@media (max-width: 768px) {
  .todo-list-container {
    margin: 20px;
    padding: 20px;
  }
  
  h1 {
    font-size: 2.2rem;
  }
  
  .add-todo-form {
    flex-direction: column;
    gap: 12px;
  }
  
  .filter-buttons {
    flex-wrap: wrap;
    gap: 8px;
  }
  
  .filter-btn {
    padding: 10px 16px;
    font-size: 13px;
  }
}
</style>