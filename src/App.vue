<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-3" v-for="col in colNames" :key="col.status">
        <StatusCard :col="col" :tasks="filteredTasks(col.status)" @new-task="addTask" @status-updated="updateStatus" @delete-task="deleteTask"></StatusCard>
      </div>
    </div>
  </div>
</template>

<script>
import StatusCard from "./components/StatusCard";

export default {
  name: "App",
  components: {
    StatusCard,
  },
  data() {
    return {
      tasks: [
        {
          id: 1,
          content: "Dashboard überarbeiten.",
          status: 0,
        },
        {
          id: 2,
          content: "Anwendung auf Vue.js umstellen.",
          status: 0,
        },
        {
          id: 3,
          content: "Kan-Ban-Board fertigstellen.",
          status: 1,
        },
        {
          id: 4,
          content: "Cool sein.",
          status: 2,
        },
        {
          id: 5,
          content: "Kan-Ban-Board Dark Style implementieren.",
          status: 3,
        },
      ],
      colNames: [
        {
          name: "Neu",
          color: "danger",
          newTask: true,
          status: 0,
        },
        {
          name: "In Bearbeitung",
          color: "info",
          newTask: false,
          status: 1,
        },
        {
          name: "Überprüfen",
          color: "warning",
          newTask: false,
          status: 2,
        },
        {
          name: "Fertig",
          color: "success",
          newTask: false,
          status: 3,
        }
      ]
    };
  },
  methods: {
    filteredTasks(status) {
      return this.tasks.filter((task) => task.status === status);
    },
    addTask(newTask) {
      this.tasks.push({
        id: (this.tasks.length + 1),
        content: newTask.content,
        status: newTask.status,
      })
    },
    updateStatus(statusDO) {
      const task = this.tasks.find((task) => task.id === Number(statusDO.taskId));
      task.status = statusDO.newStatus;
    },
    deleteTask(toDel) {
      const delIndex = this.tasks.findIndex((task) => task.id === toDel.taskId);
      this.tasks.splice(delIndex, 1);
    }
  }
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.min.css";
</style>
