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

  const filteredToDos = computed(()=> {
    return hideDone.value ?
    todos.value.filter( (t) => !t.done) : todos.value
  })

  function addToDo() {
    todos.value.push({ id: id++, title: newToDo.value, done: false })
    newToDo.value = ""
  }

  function removeToDo(todo) {
    todos.value = todos.value.filter( (t) => t !== todo )
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
      <span :class="{ done: todo.done }">
        {{todo.title}}
      </span>
      <button @click="removeToDo(todo)">X</button>
    </li>
  </ul>

  <button @click="hideDone=!hideDone">{{hideDone ? "Unhide Done" : "Hide Done" }}</button>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>
