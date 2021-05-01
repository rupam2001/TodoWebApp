<template>
  <div class="main">
    <div>
      <div class="navbar">
        <span class="task-left"
          >Task left: <strong>{{ taskLeft }}</strong>
        </span>
        <i
          class="fa fa-plus"
          aria-hidden="true"
          v-on:click="showCreate = !showCreate"
        ></i>
      </div>
      <div v-if="showCreate">
        <TaskCreate
          v-bind:taskData="currentTask"
          v-on:save="addNewTaskToLocalStorage"
          v-on:cancel="showCreate = !showCreate"
        />
      </div>
      <div v-if="!showCreate">
        <div v-for="todo in todoData" :key="todo._id" class="alltodos">
          <Todo
            v-bind:todo="todo"
            v-on:remove="removeFromLocalStorage"
            v-on:open-task="openTask"
            v-on:done-task="doneTask"
          />
        </div>
      </div>
      <div v-if="todoData.length == 0 && !showCreate" class="empty">
        <span>No tasks</span>
      </div>
    </div>
  </div>
</template>

<script>
import Todo from "./todo";
import TaskCreate from "./taskview";

const dbTodo = "dbTodo";

export default {
  name: "Todos",
  components: {
    Todo,
    TaskCreate,
  },
  beforeMount() {
    this.loadDataFromLocalStorage();
  },
  data() {
    return {
      todoData: [],
      showCreate: false,
      currentTask: {},
      taskLeft: 0,
    };
  },
  methods: {
    openTask(task) {
      this.currentComp = "taskView";
      this.currentTask = task;
    },
    saveOrCancel(save, task) {
      if (!save) return;
      this.addNewTaskToLocalStorage(task);
    },
    doneTask(id) {
      this.todoData = this.todoData.map((td) => {
        if (td._id == id) td.isCompleted = true;
        return td;
      });
      localStorage.setItem(dbTodo, JSON.stringify(this.todoData));
      this.taskLeft = this.todoData.filter((t) => !t.isCompleted).length;
    },
    loadDataFromLocalStorage() {
      this.todoData = sampleTodo;
      let todos = localStorage.getItem(dbTodo);
      if (!todos) {
        return;
      }
      todos = JSON.parse(todos);
      this.todoData = todos;

      this.taskLeft = todos.filter((t) => !t.isCompleted).length;
    },
    removeFromLocalStorage(id) {
      this.todoData = this.todoData.filter((t) => t._id !== id);
      let currentData = localStorage.getItem(dbTodo);
      currentData = JSON.parse(currentData);
      let finalData = currentData.filter((t) => t._id !== id);
      localStorage.setItem(dbTodo, JSON.stringify(finalData));
      this.taskLeft = this.todoData.filter((t) => !t.isCompleted).length;
    },
    addNewTaskToLocalStorage(task) {
      let currentData = localStorage.getItem(dbTodo);
      currentData = JSON.parse(currentData);
      if (!currentData) currentData = [];
      let newData = [...currentData, task];
      localStorage.setItem(dbTodo, JSON.stringify(newData));
      this.showCreate = false;
      this.todoData.push(task);
      this.taskLeft = this.todoData.filter((t) => !t.isCompleted).length;
    },
  },
};

const sampleTodo = [];
</script>
<style scoped>
.main {
  padding-bottom: 5rem;
}
.alltodos {
  width: 100%;
  justify-content: center;
  align-items: center;
  border-radius: 1rem;
}
.navbar {
  display: flex;
  justify-content: flex-end;
  padding: 0rem;
  margin-top: 0.8rem;
  margin-bottom: 1rem;
}
.task-left {
  flex: 1;
  align-self: center;
  color: rgb(163, 25, 16);
}
.fa-plus {
  background-color: rgba(178, 243, 183, 0.527);
  color: green;
  padding: 0.6rem 0.7rem 0.6rem 0.7rem;
  border-radius: 50%;
}
.empty {
  text-align: center;
  color: gray;
}
</style>