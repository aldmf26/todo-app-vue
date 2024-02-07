<template>
  <div class="relative">
    <input
      type="text"
      placeholder="+ Add new task. press enter to save"
      class="form-control form-control-lg padding-right-lg"
      @keydown.enter="addNewTask"
    />
  </div>
</template>

<script setup>
import { reactive,defineEmits } from 'vue';

const newTask = reactive({
    name:'',
    is_completed:false
})

const emit = defineEmits(['added'])

const addNewTask = event => {
    if(event.target.value.trim()) {
        newTask.name = event.target.value
        event.target.value = ""
        emit('added', newTask)
    }
}
</script>
