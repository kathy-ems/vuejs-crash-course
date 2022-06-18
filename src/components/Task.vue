<template>
    <div>
      <i @click="$emit('toggle-reminder')" :class="[task.reminder ? 'fa fa-light fa-circle' : 'fa-solid fa-circle-check']" ></i>
      {{ task.text }}
      <i @click="$emit('delete-task')" class="fas fa-times" />
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
.fas {
  display: flex;
  color: red;
  justify-content: right;
}
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
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  /* align-items: left; */
  /* justify-content: space-between; */
}
</style>
