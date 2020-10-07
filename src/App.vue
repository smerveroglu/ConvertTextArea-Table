<template>
  <div id="app">
    <app-text-area v-if="this.$store.state.isShow"></app-text-area>
    <app-tables v-else></app-tables>
  </div>
</template>

<script>
import Tables from "./components/Tables.vue";
import TextArea from "./components/TextArea.vue";

export default {
  components: {
    appTables: Tables,
    appTextArea: TextArea,
  },
};
</script>

<style>
#app {
  margin-left: 115px;
  margin-top: 75px;
}
</style>
