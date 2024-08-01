<script setup>
import { onMounted, ref } from "vue";
const name = ref("Chhinh");
const status = ref("pending");
const tasks = ref(["Task one", "Task two", "Task three"]);
const newTask = ref("");

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};

const addTask = () =>{
  if(newTask.value.trim() !== ''){
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deleteTask = (index) =>{
  tasks.value.splice(index, 1);
}

onMounted(async() =>{
  try{
    const response = await fetch("http://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  }catch(error){
    console.log("Error fetching task ");
  }
});
</script>
<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>
  <h1>Tasks:</h1>
  <ul>
    <li v-for="(task,index) in tasks" :key="task">
      <span>
        {{ task }}
      </span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <a :href="link">Click Go Google</a> <br />
  <button @click="toggleStatus">Change Status</button>
</template>
<style scoped></style>
