<template>
  <div class="card border-dark" @drop="onDrop" @dragover.prevent @dragenter.prevent>
    <div class="card-header text-center bg-black" :class="textColor">
      <h4> {{ this.col.name }} </h4>
    </div>
    <transition-group name="list">
    <div class="card-body bg-white" v-for="task in tasks" :key="task.id">
      <Task :task="task" :color="this.col.color" draggable="true" @dragstart="startDrag($event, task)" @delete-task="deleteTask"/>
    </div>
    </transition-group>
    <div class="card-footer bg-white" v-if="col.newTask">
      <NewTask @new-task="newTask"/>
    </div>
<!--    <div class="card-footer bg-white" v-else>-->
<!--    </div>-->
  </div>
</template>

<script>
import Task from "./Task";
import NewTask from "./NewTask";

export default {
  name: "StatusCard",
  emits: {
    "new-task": (task) => {
      if ("status" in task === false) {
        console.info("Brauchen status...");
        return false;
      }
      return true;
    },
    "status-updated": (statusDO) => {
      if ("newStatus" in statusDO === false) {
        console.info("Brauchen status...");
        return false;
      }
      return true;
    },
    "delete-task": true,
  },
  components: {
    Task,
    NewTask,
  },
  props: {
    col: Object,
    tasks: Object,
  },
  computed: {
    textColor() {
      return ["text-" + this.col.color];
    }
  },
  methods: {
    newTask(task) {
      task.status = this.col.status;
      this.$emit("new-task", task);
    },
    startDrag(event, task) {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("taskId", task.id);
    },
    onDrop(event) {
      const taskId = event.dataTransfer.getData("taskId");
      this.$emit("status-updated", { taskId: taskId, newStatus: this.col.status})
    },
    deleteTask(task) {
      this.$emit("delete-task", task);
    }
  }

};
</script>

<style scoped>
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
.list-enter-to,
.list-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.list-enter-active,
.list-leave-active {
  transition: all 0.7s ease;
}
.list-move {
  transition: transform 5.8s ease;
}
</style>