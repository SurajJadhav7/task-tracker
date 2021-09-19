<template>
  <div>
    <div v-for="task in tasks" :key="task.id">
      <Task
        @toggle-remainder="$emit('toggle-remainder', task.id)"
        @delete-task="$emit('delete-task', task.id)"
        :task="task"
      />
    </div>
  </div>
</template>

<script>
import Task from "./Task.vue";
export default {
  name: "Tasks",
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
};
</script>