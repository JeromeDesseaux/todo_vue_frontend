<template>
  <div id="app">
    <a-layout-content :style="{ padding: '0 50px', marginTop: '64px' }">
      <h1>My Todos</h1>
      <TodoForm />
      <TodoList />
    </a-layout-content>
  </div>
</template>

<script>
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'

export default {
  name: 'app',
  components: {
    TodoForm,
    TodoList
  }
}
</script>
