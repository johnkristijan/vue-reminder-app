<template>
  <div
    @dblclick="$emit('toggle-reminder', task.id)"
    v-bind:class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <!-- we need to add ", task" since we always want to display the task! -->
    <h3 v-if="!editMode">
      {{ task.text }}

      <!-- fas = font awesome class -->
      <i @click="clickHandler()" class="fas fa-edit"> </i>
      <i @click="$emit('delete-task', task.id)" class="fas fa-times"></i>
    </h3>

    <p>{{ task.day }}</p>

    <h3 v-if="editMode">
      <input v-model="task.text" />
      <i @click="saveTask()" class="fas fa-save"></i>
    </h3>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  data() {
    return {
      editMode: false,
    };
  },
  methods: {
    clickHandler() {
      this.editMode = true;
      //   this.$emit('toggle-update-task-text', task.id)
    },
    async saveTask() {
      this.editMode = false;
        const ID = this.task.id
        const response = await fetch(`api/tasks/${ID}`, {
        method: "PUT",
        headers: {
            "Content-type": "application/json",
        },
        body: JSON.stringify(this.task),
        });

        if (response.ok) {
            // all 2xx responses
            console.log('Task saved!')
        } else {
            // backend says "nayy"
            console.warn('Could not save this task!')
        }
    },
  },
};
</script>

<style scoped>
i.fas.fa-times::before {
  color: red;
}
i.fas.fa-edit::before {
  color: rgb(0, 0, 0);
}
input {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.update {
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
  align-items: center;
  justify-content: space-between;
}
.task i {
  justify-content: right;
}
</style>
