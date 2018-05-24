<template>
    <div class="container">
        <div class="options">
            <span class="remaining-todos">{{remaining}} task remaining</span>
            <span class="remaining-todos">
            <input class="done-todo" :checked = "!taskRemaining" type="checkbox"
            @change="checkAll">
            Complete All</span>
        </div>

        <input class="todo-input" type="text" placeholder="Add a new task"
        v-model="newToDo" @keyup.enter="addTodo">

        <transition-group enter-active-class="animated bounceInDown" leave-active-class="animated bounceOutUp">

        <todo-list-item  v-for="(todo,index) in todosFiltered" 
        v-bind:key="todo.id" :todo="todo" :index="index" :checkAll="!anyRemaining"
         @removeToDo="removeToDo" @finishedEdit="finishedEdit"/> 
        </transition-group>

        <div class="options bottom">

            <transition class="clearme" enter-active-class="animated fadeIn" 
            leave-active-class="animated fadeOut" name="fade">
            <button class="completed" v-if="completed" @click="clearCompleted">
            Clear Completed</button>
            </transition>

            <button class="all" :class="{active : filter == 'all'}"
             @click="filter = 'all'"> All</button>
            <button class="remaining"  :class="{active : filter == 'remaining'}" 
            @click="filter = 'remaining'" >Remaining</button>
            <button class="completed"  :class="{active : filter == 'completed'}"
            @click="filter = 'completed'" >Completed</button>
      
        </div>
    </div> 
</template>

<script>
import TodoListItem from "./TodoListItem";
export default {
  name: "todo-list",
  components: {
    TodoListItem
  },

  data() {
    return {
      newToDo: "",
      idForTodo: 0,
      editCache: "",
      filter: "all",
      acti: false,
      todos: [
      ]
    };
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
        this.newToDo = "";
        this.idForTodo += 1;
        this.filter = "all";
      } else {
        alert("Please add a message to your task");
      }
    },
    removeToDo(index) {
      this.todos.splice(index, 1);
    },
    checkAll() {
      this.todos.forEach((todo) => todo.done = event.target.checked);
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.done);
    },
    finishedEdit(data) {
      this.todos.splice(data.index, 1, data.todo);
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.done).length;
    },
    completed() {
      return this.todos.filter(todo => todo.done).length > 0;
    },
    taskRemaining() {
      return this.remaining != 0;
    },
    todosFiltered() {
      switch (this.filter) {
        default:
          return this.todos;
          break;
        case "remaining":
          return this.todos.filter(todo => !todo.done);
          break;
        case "completed":
          return this.todos.filter(todo => todo.done);
          break;
      }
    }
  }
};
</script>

<style lang="scss" >
@import url(https://cdn.jsdelivr.net/npm/animate.css@3.5.2/animate.min.css);
* {
  font-family: Arial, Helvetica, sans-serif;
}
.container {
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  padding: 15px;
}

.options {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #e0e0e0;

  &.bottom {
    justify-content: end;
    button {
      margin: 0px 10px;
      border: 0px;
      padding: 10px;
      border-radius: 5px;
      background: #35495e;
      color: whitesmoke;
      &.active {
        background: #42b883;
        border: 0px;
      }
    }
  }
  span {
    align-self: center;
    display: flex;
    align-items: center;
  }
}
.todo-input {
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

</style>
