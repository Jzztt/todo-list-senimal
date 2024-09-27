<script setup>
import { X } from 'lucide-vue-next'
import { ref, watch } from 'vue'

const props = defineProps(['task', 'showModal'])
const info = ref('')
const emit = defineEmits(['toggleModal', 'updateTask'])

const closeModal = () => {
  emit('toggleModal', false)
}
watch(
  () => props.task,
  (newTask) => {
    if (newTask) {
      info.value = newTask.title
    }
  }
)

const saveChanges = async () => {
  if (info.value) {
    await emit('updateTask', { ...props.task, title: info.value })
    closeModal()
  }
}
</script>
<template>
  <div>
    <div
      class="modal fade"
      tabindex="-1"
      role="dialog"
      :class="{ show: showModal }"
      :style="{ display: showModal ? 'block' : 'none' }"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Sửa thông tin</h5>
            <X @click="closeModal" />
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="info">Title</label>
                <input type="text" class="form-control" id="info" v-model="info" />
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" @click="closeModal">Đóng</button>
            <button type="button" class="btn btn-primary" @click="saveChanges">Lưu thay đổi</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="showModal" class="modal-backdrop fade show"></div>
  </div>
</template>



<style scoped>
.modal-header {
  display: flex;
  justify-content: space-between;
}
</style>
