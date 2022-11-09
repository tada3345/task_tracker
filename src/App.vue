<template>
  <div class="container">
    <Header
      title="タスクトラッカー"
      @toggle-add-task="toggleAddTask"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @delete-task="deleteTask"
      :tasks="tasks"
      @toggle-task="toggleReminder"
    />
  </div>
</template>

<script>
import AddTask from './components/AddTask';
import Header from './components/Header';
import Tasks from './components/Tasks';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    addTask(task) {
      console.log(task);
      this.tasks = [...this.tasks, task];
    },
    deleteTask(id) {
      if (confirm('本当に削除しますか')) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleReminder(id) {
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !task.reminder } : task
      );
    },
  },
  created() {
    this.tasks = [
      {
        id: 1,
        text: '病院',
        day: '3月1日 13:00',
        reminder: true,
      },
      {
        id: 2,
        text: '歯医者',
        day: '3月3日 15:00',
        reminder: true,
      },
      {
        id: 3,
        text: '美容院',
        day: '3月4日 16:00',
        reminder: false,
      },
    ];
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
</style>
