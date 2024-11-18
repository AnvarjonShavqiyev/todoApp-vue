<template>
  <div class="space-y-4 p-4 border border-gray-300 bg-white rounded-xl shadow-lg max-w-lg mx-auto w-[400px]">
    <form @submit.prevent="handleAddTask">
    <div class="flex items-center space-x-4">
      <div class="flex-grow">
        <label for="taskName" class="block text-[16px] font-semibold text-gray-700">Task Name</label>
        <input 
          v-model="taskFormValues.taskName" 
          type="text" 
          id="taskName" 
          placeholder="Enter your task here..."
          class="w-full p-2 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>
      <button 
        type="submit" 
        class="w-12 h-12 bg-blue-500 text-white rounded-full hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all text-lg font-bold"
      >
        +
      </button>
    </div>
    </form>
    <div class="flex gap-2">
      <select @change="handleSortByOption" v-model="sortType" class="border rounded-md bg-white text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
        <option value="A-Z" class="text-gray-600">A-Z</option>
        <option value="Z-A" class="text-gray-600">Z-A</option>
      </select>
      <select v-model="deleteOption" class="border rounded-md bg-white text-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-300">
        <option value="all" class="text-gray-600">Delete All</option>
        <option value="completed" class="text-gray-600">Delete Completed Tasks</option>
      </select>
      <button @click="handleDeleteTask" class="px-2 bg-red-500 text-white rounded-md hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-300">
        Delete 
      </button>
    </div>

  </div>
</template>

<script>
export default {
  name: 'TaskForm',
  data() {
    return {
      taskFormValues: {
        taskName: '',
        status: false
      },
      sortType: localStorage.getItem('sortType') || 'A-Z',
      deleteOption: 'all'
    };
  },
  methods: {
    handleAddTask() {
      const newTask = { ...this.taskFormValues };
      this.$emit('add-task', newTask);
      this.taskFormValues.taskName = '';
    },
    handleDeleteTask(){
      this.$emit('delete-tasks', this.deleteOption)
    },
    handleSortByOption(event){
      const type = event.target.value
      localStorage.setItem('sortType', type)
      this.$emit('sort-tasks', type)
    }
  },
};
</script>
