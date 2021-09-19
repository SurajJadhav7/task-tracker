<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
      @btn-click="this.$emit('toggle-add-task')"
    />
  </header>
</template>

<script>
import Button from "./Button.vue";
export default {
  name: "header",
  components: {
    Button,
  },
  props: {
    title: {
      type: String,
      default: "Title",
    },
    showAddTask: Boolean,
  },
  computed: {
    homePage() {
      if (this.$route.path === "/") {
        return true;
      }
      return false;
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>