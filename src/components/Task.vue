<template>
  <div class="bg-gray-100 border-gray-400 border-b px-3 py-1 ">
    <div class="grid gap-2 grid-cols-8 items-center">
      <span class="justify-self-start pl-2">
        <i @click="$emit('toggle-reminder')" :class="[task.reminder ? 'fa fa-light fa-circle' : 'fa-solid fa-circle-check']"></i>
      </span>
      <span :class="[task.reminder ? '' : 'line-through text-grayBlue-200', 'col-start-2 col-end-7']">
        {{ task.text }}
      </span>
      <span class="col-start-8 col-span-1 justify-self-end pr-2">
        <i @click="$emit('delete-task')" class="fas fa-times" />
      </span>
    </div>
  </div>
</template>

<script>
export default {
    name: 'Task',
    props: {
        task: Object
    },
}
</script>

<style scope>
/* .fas {
  display: flex;
  color: red;
  justify-content: right;
} */
.fa-circle-check {
  background: -webkit-gradient(linear, left top, right bottom, from(hsl(192, 100%, 67%)), to(hsl(280, 87%, 65%)));
	-webkit-background-clip: text;
	-webkit-text-fill-color: transparent;
}
.fa-circle {
  background: -webkit-gradient(linear, left top, right bottom, from(hsl(192, 100%, 67%)), to(hsl(280, 87%, 65%)));
	-webkit-background-clip: text;
	-webkit-text-fill-color: transparent;
}
/* .task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
} */
.task.reminder {
  /* border-left: 5px solid green; */
}
.task h3 {
  display: flex;
  /* align-items: left; */
  /* justify-content: space-between; */
}
</style>
