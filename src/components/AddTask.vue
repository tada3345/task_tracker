<template>
  <form @submit.prevent="onSubmit" class="add-form">
    <div class="form-control">
      <label for="">タスク</label>
      <input
        v-model="text"
        type="text"
        name="text"
        placeholder="タスクを追加する"
      />
    </div>
    <div class="form-control">
      <label for="">日時</label>
      <input v-model="day" type="text" name="day" placeholder="日時" />
    </div>
    <div class="form-contorl form-control-check">
      <label for="">リマインダー設定 </label>
      <input v-model="reminder" type="checkbox" name="reminder" />
    </div>
    <div class="form-control">
      <input type="submit" value="セーブする" class="btn btn-block" />
    </div>
  </form>
</template>

<script>
export default {
  name: 'AddTask',
  data() {
    return {
      text: '',
      day: '',
      reminder: false,
    };
  },
  methods: {
    onSubmit() {
      if (!this.text) {
        alert('please add a task');
        return;
      }

      const newTask = {
        // id: Math.floor(Math.random() * 1000),
        text: this.text,
        day: this.day,
        reminder: this.reminder,
      };
      this.$emit('add-task', newTask);
      this.text = '';
      this.day = '';
      this.reminder = false;
    },
  },
};
</script>

<style>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
