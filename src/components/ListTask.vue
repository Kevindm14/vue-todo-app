<template>
  <div class="list-tasks">
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <p v-show="task.completed == true">Completed</p>
        <button v-show="task.completed == false" id="completeTask" @click="completeTask(task)" class="btn-title">{{ task.title }}</button>
        <button v-show="task.completed == true" class="btn-delete" @click="deleteTask(task.id, index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  name: 'ListTask',
  props: ['tasks', 'title'],
  methods: {
    addTask () {
      this.$axios.post('http://todo-app-rails-api.herokuapp.com/tasks', { title: this.title })
    },
    deleteTask (id, index) {
      this.$axios.delete(`http://todo-app-rails-api.herokuapp.com//tasks/${id}`)
        .then(res => this.tasks.splice(index, 1))
    },
    completeTask (task) {
      task.completed = !task.completed
    }
  }
}
</script>

<style scoped>
  .list-tasks {
    width: 100%;
  }

  ul {
    padding: 0;
  }

  .btn-delete {
    width: 20%;
    padding: 10px 0;
    cursor: pointer;
    color: white;
    background-color: #d50000;
    display: inline-block;
    border: 1px solid #ccc;
    box-sizing: border-box;
    outline: none;
  }

  .btn-delete:hover {
    background-color: #f44336
  }

  .btn-title {
    width: 100%;
    padding: 10px 10px;
    cursor: pointer;
    color: black;
    text-align: left;
    background-color: #ccc;
    display: inline-block;
    border: 1px solid #ccc;
    box-sizing: border-box;
    outline: none;
  }

  .list-tasks ul li {
    display: flex;
    justify-content: space-between;
    list-style: none;
    padding-inline-start: 0;
    margin: 10px 0;
  }
</style>
