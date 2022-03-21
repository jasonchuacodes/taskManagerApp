<template>
<div class="main-view">
  <div class="container w-50">
    <h2 class="text-center pt-5">
      TO DO LIST APP
    </h2>
    <div class="d-flex mt-5 justify-content-center">
      <input
        v-model="taskInput"
        @keyup.enter="addTask"
        ref="taskInput"
        type="text"
        class="form-control me-3 w-75"
        placeholder="Input task here.."
      >
      <button @click="addTask(index)" class="btn btn-warning text-nowrap">Submit</button>
    </div>
    <div class="task-view container mt-5 br-2 pt-2">
      <div v-if="!this.tasks.length>0" class="text-center py-2"><h6>Yay! You don't have any more tasks.</h6></div>
      <div
        v-if="this.tasks.length>0"
        v-for="(task, i) in tasks"
        :key="i"
        class="main d-flex flex-column flex-lg-row pb-2"
      >
        <div class="col mb-1 me-lg-auto" :class="{ done: this.tasks[i].status }">
          {{task.message}}
        </div>
        <div class="col d-flex flex-row gap-1 justify-content-end overflow-hidden">
          <button @click="updateStatus(i)" class="btn btn-success" :class=" {updated: this.tasks[i].status} ">
            {{task.statusMessage }}
          </button>
          <button @click="editTask(i)" class="btn btn-warning fa-solid fa-pen"></button>
          <button @click="deleteTask(i)" class="btn btn-secondary fa-solid fa-trash"></button>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'TodoApp',
  data() {
    return {
      tasks: [],
      taskInput: "",
      isEditing: null,
      
    }
  },
  methods: {
    addTask() {
      if (this.taskInput == "") return

      if (this.isEditing === null) {
        this.tasks.push({message: this.taskInput, statusMessage: 'Pending'})
        this.taskInput = ""
      }else {
        this.tasks[this.isEditing].message = this.taskInput
        this.isEditing = null
        this.taskInput = ""
      }
    },
    deleteTask(index) {
      this.tasks.splice(index,1)
    },
    editTask(index) {
      this.taskInput = this.tasks[index].message
      this.isEditing = index
      this.$refs.taskInput.focus()
    },
    updateStatus(index) {
      this.tasks[index].status = !this.tasks[index].status
      if(this.tasks[index].status == true) {
        this.tasks[index].statusMessage = "Done"
      } else {
        this.tasks[index].statusMessage = "Pending"
      }
    }
  }
}
</script>

<style scoped>
@import '@/assets/style.css';

.main-view {
  background-color: rgb(240, 217, 184);
  height: 100vh;
}
.main {
  align-items: center;
}
.task-view {
  background-color: rgb(240, 232, 208);
}
.done {
  color: rgb(117, 117, 117);
}
.updated {
  background-color: rgb(137, 189, 137);
  border-color: rgb(100, 133, 107);
}
</style>