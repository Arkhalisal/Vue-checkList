<script setup>
import { onMounted, ref } from "vue";
import { PlusIcon, XIcon } from "lucide-vue-next";

const tasks = ref(["Learn Vue.js", "Learn MySQL"]);
const newTask = ref("");

const addItem = () => {
  if (newTask.value.trim()) {
    tasks.value.push(newTask.value.trim());
    newTask.value = "";
  }
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

const deleteAllTask = () => {
  tasks.value = [];
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

onMounted(() => {
  let localTask = JSON.parse(localStorage.getItem("tasks"));
  if (localTask.length > 1) {
    tasks.value = localTask;
  }
});
</script>

<template>
  <div class="bg-white shadow-lg rounded-lg w-full max-w-md mx-auto p-10 relative">
    <div class="flex flex-row justify-between">
      <h1 class="text-3xl font-bold text-gray-800 mb-6">My Tasks</h1>
      <button
        v-if="tasks.length > 1"
        class="px-2 py-1 bg-red-500 rounded-lg hover:bg-red-600 transition-colors duration-200 h-1/2"
        @click="deleteAllTask()"
      >
        delete all task
      </button>
    </div>

    <div class="mb-4">
      <div class="flex items-center">
        <input
          v-model="newTask"
          @keyup.enter="addItem"
          placeholder="Add a new task"
          class="flex-grow px-4 py-2 text-gray-700 bg-gray-100 rounded-l-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
        <button
          @click="addItem"
          class="px-4 py-2 bg-blue-500 text-white rounded-r-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
        >
          <PlusIcon class="w-5 h-5" />
        </button>
      </div>
    </div>

    <ul class="space-y-2 mb-2">
      <li
        v-for="(task, index) in tasks"
        :key="index"
        class="flex items-center justify-between p-3 bg-gray-50 rounded-lg group hover:bg-gray-100 transition-colors duration-200"
      >
        <span class="text-gray-800">{{ task }}</span>
        <button @click="deleteTask(index)" class="text-gray-400 hover:text-red-500 focus:outline-none">
          <XIcon class="w-5 h-5" />
        </button>
      </li>
    </ul>

    <p v-if="tasks.length === 0" class="text-center text-gray-500 mt-4">No tasks yet. Add one above!</p>
  </div>
</template>
