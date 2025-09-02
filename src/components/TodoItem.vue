<template>
  <div class="todo-item" :class="{ completed: props.todo.completed }">
    <input 
      type="checkbox" 
      :checked="props.todo.completed" 
      @change="toggleComplete"
      class="todo-checkbox"
    />
    <span class="todo-text">{{ props.todo.text }}</span>
    <button @click="deleteTodo" class="delete-btn">×</button>
  </div>
</template>

<script setup lang="ts">
interface Todo {
  id: number
  text: string
  completed: boolean
}

interface Props {
  todo: Todo
}

const props = defineProps<Props>()

const emit = defineEmits<{
  toggle: [id: number]
  delete: [id: number]
}>()

const toggleComplete = () => {
  emit('toggle', props.todo.id)
}

const deleteTodo = () => {
  emit('delete', props.todo.id)
}
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  padding: 16px 20px;
  background: white;
  border: none;
  border-radius: 16px;
  margin-bottom: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.todo-item::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 100%;
  background: #007bff;
  border-radius: 2px 0 0 2px;
}

.todo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

.todo-item.completed {
  background: #f8f9fa;
  transform: none;
}

.todo-item.completed::before {
  background: #28a745;
}

.todo-item.completed:hover {
  transform: none;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

.todo-checkbox {
  position: relative;
  margin-right: 16px;
  width: 22px;
  height: 22px;
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  border: 2px solid #dee2e6;
  border-radius: 6px;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  background: white;
}

.todo-checkbox:hover {
  border-color: #007bff;
  transform: scale(1.05);
}

.todo-checkbox:checked {
  background: #007bff;
  border-color: #007bff;
}

.todo-checkbox:checked::after {
  content: '✓';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 14px;
  font-weight: bold;
}

.todo-text {
  flex: 1;
  font-size: 16px;
  font-weight: 500;
  color: #495057;
  transition: all 0.3s ease;
  line-height: 1.5;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #6c757d;
  opacity: 0.7;
}

.delete-btn {
  background: #dc3545;
  color: white;
  border: none;
  border-radius: 12px;
  width: 36px;
  height: 36px;
  cursor: pointer;
  font-size: 18px;
  font-weight: bold;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 8px rgba(255, 107, 107, 0.3);
}

.delete-btn:hover {
  background: #c82333;
  transform: scale(1.1) rotate(90deg);
  box-shadow: 0 4px 12px rgba(255, 82, 82, 0.4);
}

.delete-btn:active {
  transform: scale(0.95) rotate(90deg);
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.todo-item {
  animation: slideIn 0.3s ease-out;
}
</style>