<template>
  <div>
    <Todos />
  </div>
</template>

<script>
import Todos from "./components/todos";
import AOS from "aos";
import "aos/dist/aos.css";
export default {
  name: "App",
  components: { Todos },
  created() {
    AOS.init();
  },
  beforeMount() {},
  data() {},
  methods: {},
};
</script>

<style>
#app {
  font-family: Arial;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
body {
  -webkit-user-select: none;
}
::-webkit-scrollbar {
  display: none;
}
</style>
