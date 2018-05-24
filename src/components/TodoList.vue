<template>
    <div class="container">
        <div class="options">
        <span class="remaining-todos">{{remaining}} task remaining</span>
        <span class="remaining-todos">
        <input class="done-todo" :checked = "!taskRemaining" type="checkbox"
        @change="checkAll(todo, $event)">
        Complete All</span>
</div>

        <input class="todo-input" type="text" placeholder="Add a new task"
        v-model="newToDo" @keyup.enter="addTodo">
        <div class="todo"  v-if="!todo.editing" v-for="(todo,index) in todos" v-bind:key="todo.id" @dblclick="editToDo(todo)"> 
            <input class="done-todo" type="checkbox" v-model="todo.done">
            <span :class="{doneTask:todo.done}" >{{todo.title}}</span>
            <span class="remove-todo" @click="removeToDo(index)" > X </span>  
        </div>
        <input type="text" v-else class="edit-todo"  v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
    </div> 
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      idForTodo: 0,
      editCache: "",
      todos: [
        {
          id: 1,
          title: "Title a",
          done: false,
          editing: false
        },
        {
          id: 2,
          title: "Title b",
          done: true,
          editing: false
        }
      ]
    };
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTodo: function() {
      if (this.newToDo.trim().length > 0) {
        this.todos.push({
          id: this.idForTodo,
          title: this.newToDo,
          done: false,
          editing: false
        });
        this.newTodo = "";
        this.idForTodo += 1;
      } else {
        alert("Please add a message to your task");
      }
    },

    removeToDo(index) {
      this.todos.splice(index, 1);
    },
    editToDo(todo) {
      todo.editing = true;
      this.editCache = todo.title;
    },
    doneEdit(todo) {
      todo.editing = false;
      if (todo.title.trim().length == 0) {
        todo.title = this.editCache;
        alert("You cannot leave an empty task!");
      }
    },
    cancelEdit(todo) {
      todo.title = this.editCache;
      todo.editing = false;
    },
    checkAll(todo, event) {
      console.log(event.target.checked);
      this.todos.forEach(todo => (todo.done = event.target.checked));
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.done).length;
    },
    taskRemaining() {
      return this.remaining != 0;
    }
  }
};
</script>

<style lang="scss" scoped>
* {
  font-family: Arial, Helvetica, sans-serif;
}
.container {
  border: 1px solid #e0e0e0;
}

.options {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #e0e0e0;
  margin: 5px;
  span {
    align-self: center;
    display: flex;
    align-items: center;
  }
}
.todo-input {
  border-radius: 0px;
  padding: 15px;
  margin: 15px;
  font-size: 20px;

  &:focus {
    outline: 0;
  }
}
.edit-todo {
  grid-row: 2/3;
  grid-column: 1/-1;
  margin: 15px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 24px;
  border: 0px;
}

.todo {
  margin: 15px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 24px;
  display: grid;
  grid-auto-columns: min-content;

  grid-template-columns: repeat(3, 1fr);
  .remove-todo {
    grid-column: 3/4;
    justify-self: end;
  }
  .done-todo {
    justify-self: start;
  }

  .doneTask {
    text-decoration: line-through;
    opacity: 0.5;
  }
}
</style>
