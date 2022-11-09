<template>
  <AddTask @add-task="addTask" v-show="showAddTask" />
  <Tasks
    @delete-task="deleteTask"
    :tasks="tasks"
    @toggle-task="toggleReminder"
  />
</template>
<script>
import Tasks from '../components/Tasks';
import Task from '../components/Task';

import AddTask from '../components/AddTask';
export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    Task,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      // console.log(task);
      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm('本当に削除しますか')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('エラーが発生しました。');
      }
    },
    async toggleReminder(id) {
      const tasksToToggle = await this.fetchTask(id);
      const updTask = { ...tasksToToggle, reminder: !tasksToToggle.reminder };
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updTask),
      });

      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
