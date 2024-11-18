<template>
  <div class="space-y-4 p-2 border rounded-lg shadow-md max-w-lg mx-auto w-[400px] mt-[15px] overflow-y-auto h-[375px]">
    <div v-if="tasks.length === 0" class="text-center text-gray-500 h-[100%] flex items-center justify-center">
      No tasks available
    </div>
    <div v-for="task in tasks" :key="task.taskName" class="flex justify-between items-center p-2 border rounded-lg shadow-md mx-auto w-full mt-[15px]">
      <div class="flex gap-2 w-[90%]">
        <input type="checkbox" v-model="task.status" @click="changeTaskStatus($event, task.taskName)">
        <h2 :class="task.status ? 'font-semibold line-through text-white bg-green-500 p-[4px] rounded-[12px] w-[100%]' : 'font-semibold'">{{ task.taskName }}</h2>
      </div>
      <button @click="deleteTask(task.taskName)" class="text-[14px] p-2 bg-red-500 text-white rounded-md hover:bg-red-600 focus:outline-no">
        <Icon icon="fluent:delete-32-filled" />
      </button>
    </div>
  </div>
</template>

<script>
import { Icon } from '@iconify/vue';

export default {
  name: 'TaskList',
  components: {
    Icon,
  },
  props: {
    tasks: {
      type: Array,
      required: true,
    },
  },
  methods: {
    deleteTask(taskName) {
      this.$emit('delete-task', taskName);
    },
    changeTaskStatus(event, taskName) {
      const isChecked = event.target.checked
      this.$emit('change-status', {
        taskName, isChecked
      })
    }
  },
};
</script>
