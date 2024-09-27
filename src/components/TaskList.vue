<template>
  <div>
    <table class="table mt-5 table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.id">
          <td>
            <span>{{ task.title }}</span>
          </td>
          <td @click="changeStatus(task)" style="cursor: pointer">
            {{ task.status }}
          </td>
          <td class="text-center">
            <PencilLine @click="editTask(task)" />
          </td>
          <td class="text-center" @click="handleDeleteTask(task.id)"><Trash /></td>
        </tr>
      </tbody>
    </table>
    <TaskEditModal
      :task="selectedTask"
      :showModal="showModal"
      @toggleModal="toggleModal"
      @updateTask="updateTask"
    />
  </div>
</template>

<script setup>
import { taskService } from '@/services/taskService'
import { PencilLine, Trash } from 'lucide-vue-next'
import { ref } from 'vue'
import TaskEditModal from './TaskEditModal.vue'

const props = defineProps(['tasks', 'getTasks'])
const emit = defineEmits()

const showModal = ref(false)

const selectedTask = ref(null)

const toggleModal = (status) => {
  showModal.value = status
}

const editTask = (task) => {
  selectedTask.value = task
  toggleModal(true)
}

const updateTask = async (updateTask) => {
  await taskService.updateTask(updateTask)
  editTask.value = null
  await props.getTasks()
}
const handleDeleteTask = async (taskId) => {
  const isConfirmed = window.confirm('Are you sure you want to delete this task?')
  if (isConfirmed) {
    await taskService.deleteTask(taskId)
    await props.getTasks()
  }
}
const changeStatus = async (task) => {
  if (task.status === 'to-do') {
    task.status = 'in-progress'
  } else if (task.status === 'in-progress') {
    task.status = 'done'
  } else {
    task.status = 'to-do'
  }

  await taskService.updateTask(task)
  await props.getTasks()
}
</script>

<style lang="scss" scoped>
</style>