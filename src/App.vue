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
  <div class="min-h-screen bg-pink-50 flex flex-col items-center py-10 px-4 relative overflow-hidden">
    <div class="absolute inset-0 pointer-events-none overflow-hidden opacity-10">
      <svg class="absolute top-0 right-0 w-64 h-64 text-pink-300" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="currentColor" d="M100 10C80 10 60 30 60 50C60 70 80 90 100 90C120 90 140 70 140 50C140 30 120 10 100 10Z"></path>
        <path fill="currentColor" d="M10 100C10 80 30 60 50 60C70 60 90 80 90 100C90 120 70 140 50 140C30 140 10 120 10 100Z"></path>
        <path fill="currentColor" d="M110 100C110 80 130 60 150 60C170 60 190 80 190 100C190 120 170 140 150 140C130 140 110 120 110 100Z"></path>
        <path fill="currentColor" d="M60 150C60 130 80 110 100 110C120 110 140 130 140 150C140 170 120 190 100 190C80 190 60 170 60 150Z"></path>
        <circle fill="currentColor" cx="100" cy="100" r="30"></circle>
      </svg>
      <svg class="absolute bottom-0 left-0 w-64 h-64 text-pink-200" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
        <path fill="currentColor" d="M100 10C80 10 60 30 60 50C60 70 80 90 100 90C120 90 140 70 140 50C140 30 120 10 100 10Z"></path>
        <path fill="currentColor" d="M10 100C10 80 30 60 50 60C70 60 90 80 90 100C90 120 70 140 50 140C30 140 10 120 10 100Z"></path>
        <path fill="currentColor" d="M110 100C110 80 130 60 150 60C170 60 190 80 190 100C190 120 170 140 150 140C130 140 110 120 110 100Z"></path>
        <path fill="currentColor" d="M60 150C60 130 80 110 100 110C120 110 140 130 140 150C140 170 120 190 100 190C80 190 60 170 60 150Z"></path>
        <circle fill="currentColor" cx="100" cy="100" r="30"></circle>
      </svg>
    </div>

    <div class="w-full max-w-md bg-white rounded-lg shadow-lg p-6 z-10 transform transition-all duration-300 hover:scale-102 h-96 flex flex-col">
      <h1 class="text-3xl font-bold text-pink-500 mb-4 text-center animate-pulse">Task Manager</h1>
      
      <div class="flex mb-3">
        <input 
          type="text" 
          v-model="newTask" 
          @keyup.enter="addTask" 
          placeholder="Add a new task" 
          class="flex-1 border border-pink-300 rounded-l-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-pink-400"
        />
        <button 
          @click="addTask" 
          class="bg-pink-500 text-white px-4 py-2 rounded-r-lg hover:bg-pink-600 transition-colors duration-300"
        >
          Add
        </button>
      </div>
      
      <div class="mb-3">
        <select 
          v-model="filter"
          class="w-full border border-pink-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-pink-400 bg-white"
        >
          <option value="all">All</option>
          <option value="completed">Completed</option>
          <option value="active">Active</option>
        </select>
      </div>
      
      <div class="bg-pink-50 rounded-lg p-2 flex-1 overflow-y-auto">
        <ul class="space-y-2">
          <li 
            v-for="task in filteredTasks" 
            :key="task.id"
            class="bg-white p-3 rounded-lg shadow border border-pink-100 flex items-center transition-all duration-300 hover:shadow-md"
          >
            <input 
              type="checkbox" 
              :checked="task.completed" 
              @change="toggle(task)"
              class="mr-3 h-5 w-5 text-pink-500 rounded focus:ring-pink-400"
            />
            <span 
              :class="{'line-through text-pink-300': task.completed, 'text-gray-800': !task.completed}"
              class="flex-1 transition-all duration-300"
            >
              {{ task.text }}
            </span>
            <button 
              @click="deleteTask(task)"
              class="text-pink-500 hover:text-pink-700 transition-colors duration-300 ml-2 p-1"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
              </svg>
            </button>
          </li>
        </ul>
        
        <div v-if="filteredTasks.length === 0" class="text-center py-8 text-pink-400">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 mx-auto mb-3 animate-bounce" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
          </svg>
          <p>No tasks yet. Add one above!</p>
        </div>
      </div>
    </div>

    <svg class="absolute bottom-4 right-4 w-24 h-24 text-pink-200" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
      <path fill="currentColor" d="M50 10C45 10 40 15 40 20C40 25 45 30 50 30C55 30 60 25 60 20C60 15 55 10 50 10Z"></path>
      <path fill="currentColor" d="M20 40C15 40 10 45 10 50C10 55 15 60 20 60C25 60 30 55 30 50C30 45 25 40 20 40Z"></path>
      <path fill="currentColor" d="M80 40C75 40 70 45 70 50C70 55 75 60 80 60C85 60 90 55 90 50C90 45 85 40 80 40Z"></path>
      <path fill="currentColor" d="M40 80C40 75 45 70 50 70C55 70 60 75 60 80C60 85 55 90 50 90C45 90 40 85 40 80Z"></path>
      <circle fill="currentColor" cx="50" cy="50" r="10"></circle>
    </svg>
  </div>
</template>

<style scoped>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

li {
  animation: fadeIn 0.5s ease-out;
}

.hover\:scale-102:hover {
  transform: scale(1.02);
}
</style>