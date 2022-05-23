<template>
  <div id="app" class="container-fluid">
    <AppTitle title="VTODO" />

    <TaskForm @submit-task="addTask"></TaskForm>

    <div class="container">
      <div class="row">
        <p class="message" v-if="tasks.length === 0">
          There are no tasks, try adding one!!!
        </p>
      </div>
    </div>
    <div v-if="tasks.length > 0">
      <div class="container tasks-list">
        <div v-for="task in tasks" :key="task.id">
          <div class="row">
            <div class="col-sm-12" v-if="task.isEditMode">
              <TaskForm @submit-task="editTask"></TaskForm>
            </div>
          </div>
          <div class="row" v-if="!task.isEditMode">
            <div class="col-sm-10">
              <span v-if="task.priority.text == 'Life Changing'" title="Life Changing">
                <i class="bi bi-trophy-fill btn-outline-warning"></i>
              </span>
              <span v-if="task.priority.text == 'Important'" title="Important">
                <i class="bi bi-lightning-fill btn-outline-danger"></i>
              </span>
              <span v-if="task.priority.text == 'Meh'" title="Meh">
                <i class="bi bi-emoji-neutral btn-outline-secondary"></i>
              </span>
              {{ task.text }}
            </div>
            <div class="col-sm-1">
              <button class="btn" v-on:click="editTask(task.id)">
                <i class="bi bi-pencil-fill btn-outline-success"></i>
              </button>
            </div>
            <div class="col-sm-1">
              <button
                class="btn"
                v-on:click="deleteTask(task.id)"
              >
                <i class="bi bi-trash3-fill btn-outline-dark"></i>
              </button>
            </div>
          </div>
        </div>
      </div>

      <div class="container footer">
        <div class="row">
          <div class="col-sm-4">
            <button type="button" class="btn btn-secondary">
              TOTAL: <span class="badge text-bg-secondary">{{ tasks.length }}</span>
            </button>
          </div>
          <div class="col-sm-8">
            <button class="btn btn-dark col-sm-12" v-on:click="clearList">
              Clear list
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PRIORITY from './constants'
import AppTitle from "./components/AppTitle.vue";
import TaskForm from "./components/TaskForm.vue";

console.log(PRIORITY);

export default {
  name: "App",
  components: {
    AppTitle,
    TaskForm,
  },
  created() {
    window.addEventListener("beforeunload", this.saveState);
    const cachedTasks = JSON.parse(window.localStorage.getItem("todoTasks"));
    this.tasks = cachedTasks || this.proxy.tasks || [];
  },
  data() {
    return {
      tasks: [],
      selectedPriority: 1
    };
  },
  methods: {
    clearList() {
      this.tasks = [];
      window.localStorage.removeItem("todoTasks");
    },
    addTask(task) {
      console.log(task);
      task.priority = PRIORITY[task.priority]
      this.tasks.push(task);
    }, 
    editTask(id) {
      this.tasks.forEach(task => {
        if (task.id === id) {
          task.isEditMode = true;
        }
      })
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
    saveState() {
      console.log("Saving state!!!");
      if (this.tasks.length) {
        window.localStorage.setItem("todoTasks", JSON.stringify(this.tasks));
      } else {
        window.localStorage.removeItem("todoTasks");
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.container {
  max-width: 980px;
}

.message {
  text-align: center;
  padding: 100px 0;
}

.tasks-list {
  padding: 50px 0;
  max-width: 600px;
}

.footer {
  max-width: 600px;
}
</style>
