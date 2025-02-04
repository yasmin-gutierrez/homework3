<script setup>
import {ref, onMounted, watch} from 'vue'

const myArray = ref([]) 
const name = ref('')
const input_content = ref('')
const input_category = ref(null)

const addTodo = () =>{
  if(input_content.value.trim() === '' || input_category.value == null) {
    return
  }

  myArray.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
  })

  input_content.value = ''
  input_category.value = null
}

const removeTodo = (x) => {
  myArray.value = myArray.value.filter(Element => Element !== x)
} 

onMounted( () => {
  name.value = localStorage.getItem('name') || ''
  myArray.value = JSON.parse(localStorage.getItem('myArray')) || [] 
})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(myArray, (newVal) => {
  localStorage.setItem('myArray', JSON.stringify(newVal))
}, {deep: true}) 

</script>

<template>
  
  <main class="app">

    <section class="greeting">
      <h2 class="title">
        Welcome back, <input type="text" placeholder="Enter Name" v-model="name">
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE TO DO LIST</h3>
      <form @submit.prevent = "addTodo">
        <h4>What's on your to do list?</h4>
        <input type="text" placeholder="e.g., Make a Video" v-model="input_content"/>
  
        <h4>Pick a Category</h4>
        <div class="options">
          <label ><input type="radio" name="category" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label ><input type="radio" name="category" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
        <input type="submit" value="Add To Do"/>
      </form>
    </section>

    <section class="todo-list">
      <div class="list">
        <div v-for="x in myArray" :class="`todo-item ${x.done ? 'done' : 'not-done'}`" :key="x">
          
          <label>
            <input type="checkbox" v-model="x.done"/>
            <span :class="`bubble ${x.category}`"></span>
          </label>
          
          <div class="todo-content">
            <input type="text" v-model="x.content"/>
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(x)">Delete</button>
          </div>

        </div>
      </div>

    </section>

  </main>

</template>
