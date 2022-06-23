<template>
<div class="container">
  <h2 class="text-center mt-5">ToDo App</h2>

  <div class="d-flex">
    <input v-model="task" type="text" placeholder="Enter task" class="form-control">
    <button @click="createTask" class="btn btn-warning rounded-0">{{ this.editedTask === null ? 'CREATE' : 'EDIT' }}</button>
  </div>

  <table class="table table-bordered mt-5">
    <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>{{task.name}}</td>
      <td><div class="pointer" @click="updateStatus(task.id)">{{task.status}}</div></td>
      <td><div class="text-center pointer" @click="editTask(task.id)">
        <span class="fa fa-pen"></span>
      </div></td>
      <td><div class="text-center toggler"  @click="deleteTask(task.id)">
        <span class="fa fa-trash"></span>
      </div></td>
    </tr>
    </tbody>
  </table>
</div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },
  data() {
    return {
      task: '',
      editedTask: null,
      statuses: ['to-do', 'in-progress', 'completed'],
      tasks: []
    }
  },
  methods: {
    createTask() {
      if (this.task.length === 0) return null;
      if (this.editedTask === null) {
        this.tasks = [...this.tasks, {name: this.task, status: 'to-do', id: this.tasks.length}]
      } else {
        const candidate = this.tasks.find(item => item.id === this.editedTask)
        candidate.name = this.task
        this.editedTask = null
      }
      this.task = ''
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(item => item.id !== id)
    },
    editTask(id) {
      const candidate = this.tasks.find(item => item.id === id)
      this.task = candidate.name
      this.editedTask = id
    },
    updateStatus(id) {
      const candidate = this.tasks.find(item => item.id === id)
      if (candidate.status === 'completed') return
      const nextStatusIndex = this.statuses.findIndex(item => item === candidate.status)
      candidate.status = this.statuses[nextStatusIndex+1]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.pointer {
  cursor: pointer;
}
</style>
