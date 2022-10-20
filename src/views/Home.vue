<template>
    <AddTask 
        v-show="showAddTask" 
        @add-task="addTask"
    />
    <Tasks 
        @delete-task="deleteTask" 
        @toggle-reminder="toggleReminder"
        :tasks='tasks'
    />
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
  name: 'Home',
  components: {
    Tasks,
    AddTask, 
  },
  props: {
    showAddTask: Boolean,
  },
  data() {
    return {
        tasks: [],
    }
  },
  methods: {    
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json()

      this.tasks = this.tasks.map((t) => 
        t.id === id ? {...t, reminder: data.reminder} : t
      )
    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: 'DELETE',
        })

        res.status === 200 
          ? (this.tasks = this.tasks.filter((t) => t.id !== id))
          : alert('Error deleting task')     
      }
    }, 
    async addTask(task) {
      const res = await fetch('http://localhost:5000/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'          
        }, 
        body: JSON.stringify(task)
      })

      const data = await res.json()
      
      this.tasks = [...this.tasks, data]
    },
    async fetchTasks() {
      const res = await fetch('http://localhost:5000/tasks')

      const data = await res.json()

      return data
    },
    async fetchTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`)

      const data = await res.json()

      return data
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>
