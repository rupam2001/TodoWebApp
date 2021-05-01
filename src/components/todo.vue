<template>
  <div class="todo-wrapper">
    <div class="title" v-on:click="isTaskopen = !isTaskopen">
      <span class="title-text" v-bind:class="{ done: currTodo.isCompleted }">
        {{ currTodo.title }}
      </span>
      <span class="time">{{ moment(currTodo.time).calendar() }}</span>
    </div>
    <div v-if="isTaskopen" class="task-body" v-on:click="isTaskopen = false">
      <p>{{ currTodo.body }}</p>
    </div>
    <div class="toolbar">
      <i
        class="fa fa-check"
        aria-hidden="true"
        v-if="!currTodo.isCompleted"
        v-on:click="$emit('done-task', currTodo._id)"
      ></i>
      <i
        class="fa fa-trash"
        aria-hidden="true"
        v-on:click="showModel = !showModel"
      ></i>
    </div>
    <div v-if="showModel" class="model">
      <div class="model-menu">
        <h4>Are you sure?</h4>
        <div class="btns">
          <div v-on:click="onRemove('sure')" id="sure">Yes</div>
          <div v-on:click="onRemove('no')" id="no">No</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import moment from "moment";

export default {
  name: "Todo",
  props: ["todo"],
  data() {
    return {
      currTodo: this.todo,
      showModel: false,
      moment: moment,
      isTaskopen: false,
    };
  },
  methods: {
    onRemove(choice) {
      if (choice == "sure") this.$emit("remove", this.currTodo._id);
      this.showModel = false;
    },
    openTask() {
      this.$emit("open-task", this.todo);
    },
  },
};
</script>
<style scoped>
.todo-wrapper {
  padding: 0.5rem;
  background-color: rgb(244, 245, 245);
  max-width: 50rem;
  margin-bottom: 1rem;
  border-radius: 1rem;
  overflow: visible;
  animation-name: anim;
  transition: all 0.4s;
  animation-duration: 0.4s;
  border: 0.3px solid rgb(206, 206, 206);
}
@keyframes anim {
  from {
    transform: scale(0.5);
  }

  to {
    transform: scale(1);
  }
}
.title {
  display: flex;
  font-size: larger;
  margin-bottom: 1rem;
}
.title-text {
  color: rgb(19, 100, 100);
  flex: 1;
}
.task-body {
  /* border-bottom: 1px solid gray; */
  margin-bottom: 1rem;
}
.task-body p {
  font-size: small;
}
.toolbar {
  display: flex;
  justify-content: flex-end;
}
.toolbar i {
  margin-left: 1.5rem;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.4s;
  /* color: rgba(22, 31, 31, 0.884); */
}

.fa-check {
  background-color: rgba(178, 243, 183, 0.527);
  color: rgb(25, 109, 25);
  padding: 0.7rem 0.7rem 0.7rem 0.7rem;
  border-radius: 50%;
}
.fa-trash {
  background-color: rgba(241, 166, 166, 0.527);
  color: rgb(143, 37, 5);
  padding: 0.7rem 0.8rem 0.7rem 0.8rem;
  border-radius: 50%;
}

.model {
  position: absolute;
  z-index: 99999999;
  background-color: rgba(24, 24, 24, 0.562);
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.model-menu {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: #fff;
  border-radius: 1rem;
  width: 70%;
}
.model-menu .btns {
  padding: 1rem;
  /* background-color: #fff; */
  display: flex;
  width: 50%;
  justify-content: space-around;
  color: whitesmoke;
}
.btns div:nth-child(1) {
  background-color: rgb(224, 47, 47);
  padding: 0.5rem;
  min-width: 2rem;
  border-radius: 1rem;
  margin-right: 2rem;
}
.btns div:nth-child(2) {
  background-color: #41b883;
  min-width: 2rem;
  text-align: center;
  padding: 0.5rem;
  border-radius: 1rem;
}
.time {
  font-size: x-small;
  align-self: center;
  color: rgb(6, 110, 110);
}
.done {
  text-decoration-line: line-through;
}
</style>