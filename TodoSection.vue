<template>
    <div class="todo-section" @dragover.prevent @drop="onDrop">
        <h2>{{ title }}    {{ todos.length }}</h2>
        <ul>
            <li v-for="todo in todos" :key="todo.id" draggable="true" @dragstart="onDragStart(todo, $event)">
                {{ todo.text }}
            </li>
        </ul>
        <button id="newbtn" @click="toggleNewTodo">+ New</button>
        <transition name="fade">
            <div v-if="showNewTodoInput">
                <input type="text" v-model="newTodoText" />
                <button id="add" @click="addTodo">Add Todo</button>
            </div>
        </transition>
    </div>
</template>

<script>
import { ref } from 'vue'

export default {
    props: {
        title: String,
        todos: Array
    },


    setup(props, { emit }) {
        const newTodoText = ref('')
        const showNewTodoInput = ref(false)

        function addTodo() {
            if (newTodoText.value.trim() !== '') {
                emit('addTodo', { text: newTodoText.value.trim(), status: props.title })
                newTodoText.value = ''
                showNewTodoInput.value = false
            }
        }

        function toggleNewTodo() {
            showNewTodoInput.value = !showNewTodoInput.value
        }

        function onDragStart(todo, event) {
            event.dataTransfer.setData('text/plain', JSON.stringify(todo))
        }

        function onDrop(event) {
            event.preventDefault()
            const todo = JSON.parse(event.dataTransfer.getData('text/plain'))
            emit('moveTodo', { todo, status: props.title })
        }

        return {
            newTodoText,
            addTodo,
            onDragStart,
            onDrop,
            toggleNewTodo,
            showNewTodoInput
        }
    }
}
</script>