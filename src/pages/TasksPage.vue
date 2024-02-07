<template>
  <main style="min-height: 50vh; margin-top: 2rem">
    <div class="container">
      <div class="row">
        <div class="col-md-8 offset-md-2">
          <!-- Add new Task -->
          <NewTask @added="handleAddedTask" />
          <!-- List of tasks uncompleted -->
          <Tasks :tasks="uncompletedTask" />

          <!-- show toggle button -->
          <div class="text-center my-3" v-show="showToggleCompletedBtn">
            <button
              class="btn btn-sm btn-secondary"
              @click="($event) => (showCompletedTask = !showCompletedTask)"
            >
              <span v-if="!showCompletedTask">Show Complete</span>
              <span v-else>Hide Complete</span>
            </button>
          </div>
          <!-- List of tasks completed -->
          <Tasks :tasks="completedTask" :show="showCompletedTask" />
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref, computed } from "vue";
import { allTask, creteTask } from "../http/task-api";
import Tasks from "../tasks/Tasks.vue";
import NewTask from "@/tasks/NewTask.vue";

const tasks = ref([]);

onMounted(async () => {
  const { data } = await allTask();
  tasks.value = data.data;
});

const uncompletedTask = computed(() =>
  tasks.value.filter((task) => !task.is_completed)
);
const completedTask = computed(() =>
  tasks.value.filter((task) => task.is_completed)
);

const showToggleCompletedBtn = computed(
  () => uncompletedTask.value.length > 0 && completedTask.value.length > 0
);

const completedTaskIsVisible = computed(
  () => uncompletedTask.value.length === 0 && completedTask.value.length > 0
);

const showCompletedTask = ref(false);

const handleAddedTask = async (newTask) => {
  const { data: createdTask } = await creteTask(newTask);
  tasks.value.unshift(createdTask.data)
};
</script>

<style></style>
