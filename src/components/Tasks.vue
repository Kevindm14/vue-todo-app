<template>
  <div class="container">
    <div class="box-task">
      <h1 class="title"><span v-show="taskIncomplete().length > 0">Tasks ({{ taskIncomplete().length }})</span></h1>
      <h1 v-show="taskIncomplete().length == 0">There are no pending tasks</h1>
      <form @submit.prevent="addTask">
        <input class="input-text" type="text" v-model="title" placeholder="Add task">
        <button class="btn-submit" type="submit">Add task</button>
      </form>
    </div>
    <ListTask :tasks="tasks"/>
  </div>
</template>

<script>
import ListTask from './ListTask.vue'

export default {
  name: 'tasks',
  data () {
    return {
      title: '',
      url: 'https://todo-app-rails-api.herokuapp.com/tasks',
      tasks: []
    }
  },
  components: {
    ListTask
  },
  mounted () {
    this.$axios.get(this.url)
      .then(res => { this.tasks = res.data })
  },
  methods: {
    addTask () {
      this.$axios.post(this.url, { title: this.title })
        .then(res => this.tasks.push(res.data))
      this.title = ''
    },
    taskIncomplete () {
      return this.tasks.filter((task) => {
        return !task.completed
      })
    }
  }
}
</script>

<style scoped>
  .container {
    margin: 10px 30px;
  }

  .container .box-task {
    text-align: center;
  }

  .btn-submit {
    width: 20%;
    padding: 10px;
    margin: 0 4px;
    color: white;
    background-color: #1E88E5;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    outline: none;
  }

  .btn-submit:hover {
    background-color: #1976D2;
  }

  .container .box-task form .input-text {
    width: 40%;
    padding: 10px;
    margin: 8px 0;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
    outline: none;
  }
</style>
