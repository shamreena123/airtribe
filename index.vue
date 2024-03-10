<template>
    <div class="todo-container">
      <div class="todo-sections">
        <TodoSection class="status" title="Not Started" :todos="notCompletedTodos" @addTodo="addTodo"  @moveTodo="moveTodo" />
        <TodoSection class="status" title="In Progress" :todos="inProgressTodos" @addTodo="addTodo"  @moveTodo="moveTodo" />
        <TodoSection class="status" title="Completed" :todos="completedTodos" @addTodo="addTodo"  @moveTodo="moveTodo" />
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue'
  import TodoSection from '~/components/TodoSection.vue'
  
  export default {
    components: {
      TodoSection
    },
    setup() {
      const notCompletedTodos = ref([
        { id: 1, text: 'Card 4', status: 'Not Started' },
        { id: 2, text: 'Card 1', status: 'Not Started' },
        { id: 3, text: 'Card 5', status: 'Not Started' }
      ])
      const inProgressTodos = ref([
        { id: 4, text: 'Card 2', status: 'In Progress' }
      ])
      const completedTodos = ref([
        { id: 6, text: 'Card 3', status: 'Completed' }
      ]) 
  
      function addTodo({ text, status }) {
        const newTodo = { id: Date.now(), text, status }
        switch (status) {
          case 'Not Started':
            notCompletedTodos.value.push(newTodo)
            break
          case 'In Progress':
            inProgressTodos.value.push(newTodo)
            break
          case 'Completed':
            completedTodos.value.push(newTodo)
            break
          default:
            break
        }
      }
  
      function moveTodo({ todo, status }) {
        const source = getSource(todo.status)
        const destination = getDestination(status)
        source.value.splice(source.value.findIndex(t => t.id === todo.id), 1)
        destination.value.push(todo)
      }
  
      function getSource(status) {
        switch (status) {
          case 'Not Started':
            return notCompletedTodos
          case 'In Progress':
            return inProgressTodos
          case 'Completed':
            return completedTodos
          default:
            return []
        }
      }
  
      function getDestination(status) {
        switch (status) {
          case 'Not Started':
            return notCompletedTodos
          case 'In Progress':
            return inProgressTodos
          case 'Completed':
            return completedTodos
          default:
            return []
        }
      }

    
  
      return {
        notCompletedTodos,
        inProgressTodos,
        completedTodos,
        addTodo,
        moveTodo,
      }
    }
  }
  </script>

  