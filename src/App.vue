<script setup>
// import HelloWorld from './components/example.vue'
// ref 
//  :bind @listen v-model v-for :key
// function example() {}
// const emp = computed(() => {return ____})
  import { ref, computed } from 'vue'

  let id = 0
  
  const newToDo = ref('')
  const todos = ref([ 
    { id: id++, title: "Todo 1", done: false },
    { id: id++, title: "Todo 2", done: false },
    { id: id++, title: "Todo 3", done: false }
  ])
  const hideDone = ref(false)
  const editedToDo = ref()

  const filteredToDos = computed(()=> {
    return hideDone.value ?
    todos.value.filter( (t) => !t.done) : todos.value
  })

  function addToDo() {
    if (newToDo.value.length > 0) {
      todos.value.push({ id: id++, title: newToDo.value, done: false })
      newToDo.value = ""
    }
  }

  function removeToDo(todo) {
    todos.value = todos.value.filter( (t) => t !== todo )
  }

  let beforeEditCache = ""
  function editToDo(todo) {
    beforeEditCache = todo.title
    editedToDo.value = todo
  }

  function doneEdit(todo) {
    if (editedToDo.value) {
      if (editedToDo.value.title.trim().length <= 0) {
        removeToDo(todo)
      }
      editedToDo.value = null
    }
  }

  function cancelEdit(todo) {
    todo.title = beforeEditCache
    editedToDo.value = null
  }

</script>

<template>
  <h1>To Do App</h1>

  <form @submit.prevent="addToDo">
    <input v-model="newToDo" placeholder="New To Do">
    <button>Add Todo</button> 
  </form>

  <ul>
    <li v-for="todo in filteredToDos" :key="todo.id">
      <input type="checkbox" v-model="todo.done" />
      <span :class="{ done: todo.done }" @dblclick="editToDo(todo)">
        {{todo.title}}
      </span>
      <button @click="removeToDo(todo)">X</button>
      <input
            v-if="todo === editedToDo"
            type="text"
            v-model="todo.title"
            @vnode-mounted="({ el }) => el.focus()"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            @keyup.escape="cancelEdit(todo)"
          >
    </li>
  </ul>
  <p>{{ editedToDo }}</p>
  <button @click="hideDone=!hideDone">{{hideDone ? "Unhide Done" : "Hide Done" }}</button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
