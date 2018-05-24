<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo">
    
    <todo-list></todo-list>
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
* {
  box-sizing: border-box;
}

.container {
  max-width: 800px;
  margin: 0 auto;
  display: grid;
  justify-content: center;

  img {
    justify-self: center;
  }
}
</style>



