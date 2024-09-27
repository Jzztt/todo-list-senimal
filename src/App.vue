<script setup>
import { onMounted, ref } from 'vue'
import { taskService } from './services/taskService'
import TaskInput from './components/TaskInput.vue';
import TaskList from './components/TaskList.vue';


const tasks = ref([])

const addTask = async (task) => {
  await taskService.addTask(task)
  await getTasks()
}

const getTasks = async () => {
  const data = await taskService.getTasks()
  tasks.value = data
}

onMounted(() => {
  getTasks()
})
</script>

<template>
  <div class="container">
    <h2 class="mt-5 text-center">My Vue Todo App</h2>
    <TaskInput @add-task="addTask" />
    <TaskList :tasks="tasks" :getTasks="getTasks" />
  </div>
</template>

<style scoped>
input[type='text'] {
  width: 100%;
}
</style>
