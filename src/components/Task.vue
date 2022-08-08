<template>
  <div
      class="alert" :class="alertColor" @click="showEdit()"
  >
    {{ task.content }}
    <div v-if="edit">
      <br>
      <button class="bg-danger" @click="deleteObject()">Delete</button>
      <button class="bg-warning offset-3" @click="cancelDeletion()">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "TasK",
  emits: {
    "delete-task": true,
  },
  data() {
    return {
      edit: false,
      clickable: true,
    }
  },
  props: {
    task: Object,
    color: {
      type: String,
      default: "secondary",
    }
  },
  computed: {
    alertColor() {
      return ["alert-" + this.color];
    }
  },
  methods: {
    showEdit() {
      if (this.clickable === true) {
        this.edit = !this.edit;
        this.clickable = false;
      }
    },
    deleteObject() {
      if (this.clickable === false) {
        this.clickable = true;
        this.$emit("delete-task", { taskId: this.task.id});
      }
    },
    cancelDeletion() {
      this.edit = false;
      this.$nextTick(() => this.clickable = true);
    }
  }
};
</script>

<style scoped>

</style>