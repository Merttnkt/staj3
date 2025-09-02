<script setup lang="ts">
import TodoList from './components/TodoList.vue'
</script>

<template>
  <div id="app">
    <TodoList />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #f8f9fa;
  min-height: 100vh;
  position: relative;
  overflow-x: hidden;
}


#app {
  padding: 40px 20px;
  min-height: 100vh;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  position: relative;
  z-index: 1;
}

@media (max-width: 768px) {
  #app {
    padding: 20px 10px;
  }
}
</style>
