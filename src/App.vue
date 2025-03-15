<script lang="ts">
import { defineComponent } from 'vue';
import FormUser from './components/FormUser.vue';

export default defineComponent({
  name: 'App',
  components: {
    FormUser,
  },
});
</script>

<template>
  <div id="app">
    <FormUser />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 100vw;
  margin: 0 auto;
  padding: 0;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}
</style>
