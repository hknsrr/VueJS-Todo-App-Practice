<template>
  <div id="app">
    <img src="./assets/logo.png" class="logo">
    <TodoList/>
  </div>
</template>

<script>
import TodoList from "./components/TodoList";

export default {
  name: "App",
  components: {
    TodoList
  }
};
</script>

<style lang="scss">
@import "./css/variables";
@mixin box-sizing {
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
}

* {
  @include box-sizing;
}
input,
textarea,
select {
  font-family: inherit;
  font-size: inherit;
}
#app {
  font-family: $font-stack;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: $primary-color;
  margin-top: 60px;
  max-width: 600px;
  margin: 0 auto;

  .logo {
    display: block;
    margin: 0 auto;
  }
}
</style>
