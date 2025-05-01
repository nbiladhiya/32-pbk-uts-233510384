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
    newTask.value = '';
  }
};

const deleteTask = (task) => {
  tasks.value = tasks.value.filter(t => t !== task);
  console.log(tasks.value);
};

const toggle = (task) => {
  task.completed = !task.completed;
  console.log(task.completed);
}

</script>

<template>
  <h1>Task Manager</h1>
  <input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
  <button @click="addTask">Add</button>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <input type="checkbox" :checked="task.completed" @change="toggle(task)" />
      {{ task.text }}
      <button @click="deleteTask(task)">Delete</button>
    </li>
  </ul>
</template>

<style scoped></style>
