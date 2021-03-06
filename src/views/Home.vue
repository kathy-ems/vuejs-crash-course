<template>
  <div>
    <AddTask @add-task="addTask" />
    <Tasks
      @toggle-completed="toggleCompleted"
      @delete-task="deleteTask"
      @filter-items="filterItems"
      @update-order="updateOrder"
      :tasks="tasks"
      :openItems="openItems"
      :icons="icons"
      :filterLevel="filterLevel"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import Tasks from "@/components/Tasks.vue";
import AddTask from "@/components/AddTask.vue";
import iconCheck from "@/assets/icon-check.svg";
import iconCross from "@/assets/icon-cross.svg";

export default {
  name: "Home",
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      openItems: 0,
      filterLevel: "all",
    };
  },
  computed: {
    icons() {
      return {
        iconCheck,
        iconCross,
      };
    },
  },
  methods: {
    async addTask(task) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
      this.getOpenItems(this.tasks);
    },
    async toggleCompleted(id) {
      const taskToToggle = await this.fetchTask(id);
      const toggledTask = {
        ...taskToToggle,
        completed: !taskToToggle.completed,
      };
      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(toggledTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, completed: data.completed } : task
      );
      this.getOpenItems(this.tasks);
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;
    },
    async deleteTask(task) {
      const id = task.id;
      if (confirm(`Delete task ${task.text}?`)) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting task");
        this.getOpenItems(this.tasks);
      }
    },
    getOpenItems(tasks) {
      const openItems = tasks.filter((task) => task.completed === false);
      this.openItems = openItems.length;
    },
    async filterItems(filter) {
      this.tasks = await this.fetchTasks();
      if (filter === "active") {
        this.tasks = this.tasks.filter((task) => task.completed === false);
      }
      if (filter === "completed") {
        this.tasks = this.tasks.filter((task) => task.completed === true);
      }
      if (filter === "clear") {
        const toDelete = this.tasks.filter((task) => task.completed === true);
        toDelete.forEach(async (task) => {
          await this.deleteTask(task);
        });
      }
      this.filterLevel = filter === "clear" ? "all" : filter;
    },
    async fetchTasks() {
      const res = await fetch("api/tasks?_sort=order&_order=asc");
      const data = await res.json();
      return data;
    },
    async updateOrder(taskUpdateData) {
      const { oldIndex, newIndex, id, text, completed } = taskUpdateData;
      const newTask = {
        id,
        text,
        order: newIndex,
        completed,
      };
      // removes old location of task & adds task to new location
      const originalData = await this.fetchTasks();
      originalData.splice(oldIndex, 1);
      originalData.splice(newIndex, 0, newTask);

      // updates ordering; use promise.all for larger data sets
      for (let i = 0; i < originalData.length; i++) {
        const task = originalData[i];
        task.order = i;
        this.updateTask(task);
      }
    },
    async updateTask(task) {
      await fetch(`api/tasks/${task.id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
    this.getOpenItems(this.tasks);
  },
};
</script>
