<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);
const newTask = ref('');
const filter = ref('all');

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
};

const toggle = (task) => {
  task.completed = !task.completed;
}

const filteredTasks = computed(() => {
  if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed);
  } else if (filter.value === 'active') {
    return tasks.value.filter(task => !task.completed);
  } else {
    return tasks.value;
  }
})

</script>

<template>
  <h1>Task Manager</h1>
  <input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
  <button @click="addTask">Add</button>
  <select v-model="filter">
    <option value="all">All</option>
    <option value="completed">Completed</option>
    <option value="active">Active</option>
  </select>

  <ul>
    <li v-for="task in filteredTasks" :key="task.id">
      <input type="checkbox" :checked="task.completed" @change="toggle(task)" />
      {{ task.text }}
      <button @click="deleteTask(task)">Delete</button>
    </li>
  </ul>
</template>

<style scoped></style>
