<template>
  <div>
    <AddTask @add-task="addTask" v-show="showAddTask" />
    <Tasks
      @toggle-remainder="toggleRemainder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async deleteTask(id) {
      if (confirm("Are you sure you want to delete this task?")) {
        const response = await fetch(`/api/tasks/${id}`, {
          method: "DELETE",
        });
        if (response.status === 200) {
          this.tasks = this.tasks.filter((task) => task.id !== id);
        } else {
          alert("Error deleting task");
        }
      }
    },
    async toggleRemainder(id) {
      const taskToToggle = await this.tasks.find((task) => task.id === id);
      taskToToggle.showRemainder = !taskToToggle.showRemainder;
      const response = await fetch(`/api/tasks/${id}`, {
        method: "PUT",
        body: JSON.stringify(taskToToggle),
        headers: {
          "Content-Type": "application/json",
        },
      });
      const data = await response.json();
      if (response.status === 200) {
        this.tasks = this.tasks.map((task) => {
          if (task.id === id) {
            task.remainder = !data.remainder;
          }
          return task;
        });
      } else {
        alert("Error toggling remainder");
      }
    },
    async addTask(task) {
      const response = await fetch("/api/tasks", {
        method: "POST",
        body: JSON.stringify(task),
        headers: {
          "Content-Type": "application/json",
        },
      });
      const newTask = await response.json();
      this.tasks.push(newTask);
    },
    async fetchTasks() {
      const response = await fetch("api/tasks");
      const tasks = await response.json();
      return tasks;
    },
    async fetchTask(id) {
      const response = await fetch(`api/tasks/${id}`);
      const task = await response.json();
      return task;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>