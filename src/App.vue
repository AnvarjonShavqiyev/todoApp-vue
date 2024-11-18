<template>
  <TaskForm 
  @add-task="handleAddTask"
  @delete-tasks="handleDeleteTasks"
  @sort-tasks="handleSortByOption" />
  <TaskList :tasks="tasks" 
  @delete-task="handleDeleteTask" 
  @change-status="handleChangeStatus"/>
</template>

<script>
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';

export default {
  name: 'App',
  components: {
    TaskForm,
    TaskList,
  },
  data() {
    return {
      tasks: JSON.parse(localStorage.getItem('allTasks')) || [],
    };
  },
  methods: {
    handleAddTask(newTask) {
      this.tasks.push(newTask);
      localStorage.setItem('allTasks', JSON.stringify(this.tasks));
    },
    handleDeleteTask(taskName) {
      this.tasks = this.tasks.filter((task) => task.taskName !== taskName);
      localStorage.setItem('allTasks', JSON.stringify(this.tasks));
    },
    handleChangeStatus(data){
      this.tasks = this.tasks.map((task) => {
        if(task.taskName === data.taskName){
          task.status = data.isChecked
        }
        return task
      })
      localStorage.setItem('allTasks', JSON.stringify(this.tasks));
    },
    handleDeleteTasks(option){
      if (option === 'all'){
        this.tasks = []
        localStorage.setItem('allTasks', JSON.stringify(this.tasks));
      }
      if (option === 'completed') {
        console.log(option)
        this.tasks = this.tasks.filter((task) => task.status !== true)
        localStorage.setItem('allTasks', JSON.stringify(this.tasks));
      }
    },
    handleSortByOption(type){
      if (type === 'A-Z') {
        this.tasks.sort((a,b) => a.taskName.localeCompare(b.taskName))
        localStorage.setItem('allTasks', JSON.stringify(this.tasks));
      }
      if (type === 'Z-A') {
        this.tasks.sort((a,b) => b.taskName.localeCompare(a.taskName))
        localStorage.setItem('allTasks', JSON.stringify(this.tasks));
      }
    }
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
}
</style>
