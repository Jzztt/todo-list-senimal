<template>
  <div class="d-flex">
    <input
      v-model="task.title"
      type="text"
      placeholder="Enter Task"
      class="form-control"
    />
    <button @click="addTask" class="btn btn-warning rounded-0">Submit</button>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits();
const task = ref({
  title: '',
  status: 'to-do',
});

const addTask = () => {
  if (task.value.title) {
    emit('add-task', task.value);
    task.value = { title: '', status: 'to-do' };
  }
};
</script>

<style scoped>
input[type='text'] {
  width: 100%;
}
</style>
