<template>
  <div>
    <input
        type="text"
        class="form-control bg-opacity-10 bg-info border-info text-black"
        placeholder="Neue Aufgabe"
        v-model="content"
    />
    <div class="d-grid my-2">
      <button class="btn btn-success text-black" @click="submitTask()">Eintragen</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "NewTask",
  // emits: ["new-task"],
  emits: {
    "new-task": (task) => {
      if (task.content === "") {
        console.warn("New task shouldn't be empty.");
        return false;
      }
      return true;
    },
  },
  data() {
    return {
      content: "",
    }
  },
  methods: {
    submitTask() {
      this.$emit("new-task", {
        //Payload
        content: this.content,
      });
      this.content = "";
    },
  }
};
</script>

<style scoped>

</style>