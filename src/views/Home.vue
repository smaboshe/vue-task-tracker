<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />

  <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-reminder="toggleReminder"  />
</template>

<script>
  import Tasks from '../components/Tasks'
  import AddTask from '../components/AddTask'

  export default {
    name: 'Home',
    props: {
      showAddTask: Boolean,
    },
    components: {
      Tasks,
      AddTask,
    },
    data() {
      return {
        tasks: []
      }
    },
    methods: {
      async addTask(task) {
        const response = await fetch('api/tasks', {
          method: 'POST',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(task),
        })
        const data = await response.json()
        this.tasks = [ ...this.tasks, data]
      },
      async deleteTask(id) {
        if(confirm("Are you sure?")) {
          const response = await fetch(`api/tasks/${ id }`, {
            method: 'DELETE',
          })
          response.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task')
        }
      },
      async toggleReminder(id) {
        const taskToToggle = await this.fetchTask(id)
        const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
        const response = await fetch(`api/tasks/${ id }`, {
          method: 'PUT',
          headers: {
            'Content-type': 'application/json'
          },
          body: JSON.stringify(updatedTask),
        })
        const data = await response.json()
        this.tasks = this.tasks.map((task) => task.id === id ? { ...task, reminder: data.reminder } : task)
      },
      async fetchTasks() {
        const response = await fetch('api/tasks')
        const data = await response.json()
        return data
      },
      async fetchTask(id) {
        const response = await fetch(`api/tasks/${ id }`)
        const data = await response.json()
        return data
      },
    },
    async created() {
      this.tasks = await this.fetchTasks()
    },
  }
</script>
