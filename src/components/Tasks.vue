<template>
  <div>
    <div class="bg-gray-100 rounded-md shadow-gray-900 shadow-2xl py-2">
      <div :key="task.id" v-for="task in tasks">
        <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
      </div>
      <div class="p-4 text-xs">
        # of items left  | All | Active | Completed | Clear Completed
      </div>
    </div>
  </div>
</template>

<script>
import Task from './Task.vue'

export default {
    name: 'Tasks',
    props: {
        tasks: Array
    },
    components: {
        Task
    },
    emits: ['delete-task', 'toggle-reminder']

}
</script>
