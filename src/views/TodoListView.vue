<template>
  <div id="todolist">
    <h1>My Todo List</h1>
    <span>
      <input v-model="newTodo">
      <button @click="addTodo">Add item</button>
    </span>
    <br>
    <ul>
      <li v-for="todo in sortedTodos" :key="todo.id"> 
      <span :class="{ 'done-todo': todo.complete }"> {{ todo.desc }} </span>
      <button class="delete-button" @click="removeTodo(todo)">X</button>
      <button class="done-button" @click="markAsComplete(todo)"> {{ todo.complete ? "Not done" : "Done!" }}</button>
      </li>
    </ul>
    <br>
    <label><input type="checkbox" v-model="completedToBottom">Send completed tasks to bottom</label>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

let curr_id = 1

const newTodo = ref('')
const todos = ref([
  {id: 1, desc:"Learn Vue", complete: false},
  {id: 2, desc:"Conquer the Universe", complete: false}, 
  {id: 3, desc:"Square the Circle", complete: false}
])
const completedToBottom = ref(false)
const sortedTodos = computed(() => {
  return completedToBottom.value ? todos.value.toSorted((a,b) => a.complete - b.complete): todos.value
})

function addTodo() {
  curr_id++
  todos.value.push({id: curr_id, desc: newTodo.value, complete: false})
  newTodo.value = ''
}

function removeTodo(todoToRemove) {
  todos.value = todos.value.filter(t => t !== todoToRemove)
}

function markAsComplete(todo) {
  todo.complete = !todo.complete
}

</script>

<style>
#todolist {
  min-height: 100vh;
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
}
.delete-button {
  background-color: red;
  margin: 5px;
  border-radius: 50%;
}
.done-button {
  background-color: lightgreen;
  margin: 5px;
}
.done-todo {
  text-decoration: line-through;
}
input[type="checkbox"] {
  margin-right: 10px;
}
</style>
