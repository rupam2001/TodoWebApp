<template>
  <div class="wrapper" data-aos="zoom-in">
    <span ref="msgRef" class="error"></span>
    <input
      placeholder="Title"
      v-on:change="onChangeTitle"
      v-bind:value="title"
      autofocus
    />
    <textarea
      placeholder="Body"
      v-on:change="onChangeBody"
      v-bind:value="body"
      aria-multiline="true"
      rows="10"
    />
    <div class="btns">
      <div v-on:click="$emit('cancel')">cancel</div>
      <div v-on:click="onSave">save</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskView",
  props: ["taskData"],
  data() {
    return {
      task: this.taskData,
      title: "",
      body: "",
    };
  },
  methods: {
    onChangeTitle(e) {
      this.title = e.target.value;
    },
    onChangeBody(e) {
      this.body = e.target.value;
    },
    onSave() {
      if (this.title.length === 0) return;
      if (this.title.length > 30) {
        this.$refs.msgRef.innerText = "Too long";
        return;
      }

      const newTask = {
        _id: Math.floor(Math.random() * 10000),
        title: this.title,
        body: this.body,
        isCompleted: false,
        time: new Date(),
      };
      this.$emit("save", newTask);
    },
  },
};
</script>
<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-bottom: 1rem;
  border: 1px solid whitesmoke;
  padding: 1rem;
  border-radius: 1rem;
}
input {
  font-size: larger;
}
textarea,
input {
  border: none;
  margin-bottom: 1rem;
}
textarea:focus,
input:focus {
  outline: none;
}
.btns {
  display: flex;
  justify-content: flex-end;
}
.btns div:nth-child(1) {
  margin-left: 1rem;
  background-color: rgb(241, 165, 165);

  color: rgb(92, 30, 11);
  padding: 0.5rem;
  border-radius: 1rem;
  cursor: pointer;
}
.btns div:nth-child(2) {
  margin-left: 1rem;
  background-color: rgba(178, 243, 183, 0.527);
  color: rgb(25, 109, 25);
  padding: 0.5rem;
  border-radius: 1rem;
  font-weight: bold;
  cursor: pointer;
}
.error {
  color: rgb(228, 15, 15);
  font-size: small;
  margin-bottom: 0.5rem;
}
</style>