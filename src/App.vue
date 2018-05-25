<template>
  <div id="app" class="container-app">
    
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


</style>



