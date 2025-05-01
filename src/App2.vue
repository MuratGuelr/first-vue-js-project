<script setup>
import { onMounted, ref } from 'vue'

const name = ref('ConsolAktif')
const status = ref('active')
const tasks = ref(['Task One', 'Task Two', 'Task Three'])
const newTask = ref('Hey yooo')
const link = ref('www.consolaktif.com.tr')
const linkText = ref('Click for ConsolAktif')

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'pending'
  } else if (status.value === 'pending') {
    status.value = 'inactive'
  } else {
    status.value = 'active'
  }
}

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push(newTask.value)
    newTask.value = ''
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

const handleLink = () => {
  if (link.value === 'www.consolaktif.com.tr') {
    link.value = 'www.google.com.tr'
    linkText.value = 'Click for Google'
  } else {
    link.value = 'www.consolaktif.com.tr'
    linkText.value = 'Click for ConsolAktif'
  }
}

const checkClass = () => {
  if (status.value === 'active') {
    return 'active'
  } else if (status.value === 'pending') {
    return 'pending'
  } else {
    return 'inactive'
  }
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos')
    const data = await response.json()
    tasks.value = data.map((task) => task.title)
  } catch (error) {
    console.log(error.message)
  }
})
</script>

<template>
  <h1>{{ name }}</h1>
  <p>
    User is <span :class="checkClass()">{{ status }}</span>
  </p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>

  <footer>
    <!-- <a v-bind:href="link">Click for ConsolAktif</a> -->
    <a :href="link">{{ linkText }}</a>
    <button @click="handleLink">Change Link</button>

    <!--<button v-on:click="toggleStatus">Change Status</button> -->
    <button @click="toggleStatus">Change Status</button>
  </footer>
</template>

<style scoped>
h1 {
  font-size: 3rem;
  text-align: center;
}

p {
  font-size: 1.5rem;
  text-align: center;
}

.active {
  color: green;
}

.pending {
  color: grey;
}

.inactive {
  color: red;
}

form {
  display: flex;
  flex-direction: column;
  padding: 2rem;
  width: 20rem;
  text-align: center;
  margin: 0 auto;
  gap: 1rem;
}

form input {
  border: 2px solid rgb(48, 48, 48);
  padding: 1rem;
  border-radius: 0.4rem;
  background-color: #636363;
  color: #fff;
}

form button {
  padding: 0.5rem 1rem;
  border-radius: 0.4rem;
}

h3 {
  font-size: 2rem;
  text-align: center;
}

ul {
  display: flex;
  flex-direction: column;
  font-size: 1.3rem;
  gap: 0.5rem;
  margin: 0 auto;
  justify-content: center;
}

li {
  display: flex;
  gap: 1rem;
  justify-content: space-between;
  padding: 0.5rem 1rem;
  color: grey;
  background: #000;
}

li button {
  background: none;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s;
}
li button:hover {
  background: #fff;
  color: black;
  cursor: pointer;
}

button {
  background: none;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s;
  border-radius: 0.4rem;
  padding: 0.5rem;
}

footer {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  text-align: center;
  width: 10rem;
  margin: 0 auto;
  margin-top: 2rem;
}
</style>
