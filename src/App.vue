<template>
  <div id="app">
    <do-list></do-list>
  </div>
</template>

<script>
import DoList from "./components/DoList.vue";

export default {
  name: "app",
  components: { DoList },

  data() {
    return {};
  }
};
</script>

<style>
body {
  background-color: rgb(75, 75, 75);
  font-family: "Courier New", Courier, monospace;
}
</style>
