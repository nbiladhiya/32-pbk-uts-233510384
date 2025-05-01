<script setup>
import { ref } from 'vue';

const tasks = ref([]);
const newTask = ref('');

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({
      id: tasks.value.length + 1,
      text: newTask.value, 
      completed: false 
    });
    console.log(tasks.value);
    newTask.value = '';
  }
};
</script>

<template>
  <h1>Task Manager</h1>
  <input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
  <button @click="addTask">Add</button>
</template>

<style scoped>

</style>
