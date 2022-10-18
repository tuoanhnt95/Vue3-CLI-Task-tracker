<template>
   <div class="container">
      <Header title="Task Tracker"/>
      <AddTask @add-task="addTask"/>
      <Tasks 
        @delete-task="deleteTask" 
        @toggle-reminder="toggleReminder"
        :tasks='tasks'/>
   </div>
</template>

<script>
import Header from './components/Header';
import Tasks from './components/Tasks';
import AddTask from './components/AddTask';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask,    
  },
  data() {
    return {
      tasks: []
    }
  },
  methods: {
    deleteTask(id) {
      if (confirm('Are you sure?')) {
        this.tasks = this.tasks.filter((t) => t.id !== id)
      }
    }, 
    toggleReminder(id) {
      this.tasks = this.tasks.map((t) => 
        t.id === id ? {...t, reminder: !t.reminder} : t
      )
    },
    addTask(newTask) {
      this.tasks = [...this.tasks, newTask]
    }
  },
  created() {
    this.tasks = [
    {
        id: 1,
        text: 'Confirm barbecue place',
        day: '2022/10/18 at 12:30',
        reminder: true,
      },
      {
        id: 2,
        text: 'Hang out with Jonas',
        day: '2022/10/18 at 19:00',
        reminder: true,
      },
      {
        id: 3,
        text: 'Watch movie with le Chris',
        day: '2022/10/21 at 18:45',
        reminder: false,
      },
    ]
  }
}
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
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
