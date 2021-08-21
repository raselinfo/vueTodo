<template>
  <div class="container">
    <div class="todo">
      <div class="todo__logo">
        <span class="icon"><i class="fas fa-list-ul"></i></span>
      </div>
      <div class="todo__main">
        <!-- Todo Header -->
        <div class="todo__header">
          <form @submit.prevent="addTask">
            <div class="input-group">
              <input
                v-model="task"
                class="form-control"
                type="text"
                placeholder="✍️ Write ToDo....."
              />
              <button class="btn btn-danger">
                <i class="fas fa-plus"></i>
              </button>
            </div>
          </form>

          <p class="message mt-3">{{ message }}</p>
        </div>
        <!-- Todo body -->
        <div class="todo__body">
          <div
            v-for="task in tasks"
            :key="task.id"
            class="todo__body-item d-flex justify-content-between"
          >
            <div class="left">
              <span
                @click="changeStatus(task)"
                :class="{ status: task.status === 'todo' ? false : true }"
                class="right"
                ><i class="fas fa-check"></i
              ></span>
              <span class="task">{{ task.name }}</span>
            </div>
            <div class="right">
              <span @click="editTask(task)" class="text-info"
                ><i class="fas fa-edit"></i
              ></span>
              <span @click="deleteTask(task)" class="text-danger"
                ><i class="fas fa-trash-alt"></i
              ></span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      task: "",
      tasks: JSON.parse(localStorage.getItem("tasks")),
      message: "",
      isEdit: false,
      currentEditTask: null,
    };
  },
  methods: {
    addTask() {
      if (!this.isEdit) {
        if (this.task !== "") {
          this.tasks.push({
            id: this.tasks[this.tasks.length - 1]?.id
              ? this.tasks[this.tasks.length - 1].id + 1
              : 1,
            name: this.task,
            status: "todo",
          });
          this.task = "";
          this.message = "😍 List Added Successfully";
          localStorage.setItem("tasks", JSON.stringify(this.tasks));
        } else {
          this.message = "⚠️ Please write your ToDo";
        }
      } else {
        let newTask = { ...this.currentEditTask, name: this.task };
        this.tasks[this.tasks.indexOf(this.currentEditTask)] = newTask;
        this.isEdit = false;
        this.task = "";
        localStorage.setItem("tasks", JSON.stringify(this.tasks));
      }
    },
    deleteTask(task) {
      this.tasks = this.tasks.filter((item) => {
        return item.id !== task.id;
      });
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    editTask(task) {
      this.task = task.name;
      this.isEdit = true;
      this.currentEditTask = task;
    },
    changeStatus(task) {
      if (task.status.toLowerCase() === "todo") {
        this.tasks[this.tasks.indexOf(task)] = { ...task, status: "complete" };
      } else {
        this.tasks[this.tasks.indexOf(task)] = { ...task, status: "todo" };
      }
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
  },
};
</script>

<style>
.status {
  background: #dc3545 !important;
}
</style>
