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
  <section class="todoapp">
    <header class="header">
      <h1>To Do App</h1>
      <input 
        class="new-todo"
        v-model="newToDo" 
        placeholder="New To Do" 
        @keyup.enter="addToDo"
      >
    </header>
    <ul>
      <li 
        v-for="todo in filteredToDos" 
        :key="todo.id"
        :class="{ done: todo.done, editing: todo === editedToDo }"  
      >
        <div class="view">
          <input class="toggle" type="checkbox" v-model="todo.done" />
          <label @dblclick="editToDo(todo)">
            {{todo.title}}
          </label>
          <button class="destroy" @click="removeToDo(todo)">X</button>
        </div>
        <input
              v-if="todo === editedToDo"
              class="edit"
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
  </section>
</template>

<style scoped>
@import "assets/main.css";


</style>
