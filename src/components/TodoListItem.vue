<template>
    <div class="todo" v-if="!editing"   @dblclick="editToDo(todo) ">
        <input class="done-todo" type="checkbox" v-model="done" @change="doneEdit" >
        <span class="task-info"
        :class="{doneTask:done}" >{{title}}</span>
        <span class="remove-todo" @click="removeToDo(id)" > X </span>
    </div>
    <div v-else class="editing edit-todo">Editing:
        <input type="text"  class="edit-todo"
        v-model="title" @blur="doneEdit" @keyup.enter="doneEdit"
         @keyup.esc="cancelEdit" v-focus>
        </div>
</template>

<script>
export default {
  name: "todo-item",
  props: {
    todo: {
      type: Object,
      required: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },
  watch: {
    checkAll() {
      this.done = this.checkAll ? true : this.todo.done;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  data() {
    return {
      id: this.todo.id,
      title: this.todo.title,
      done: this.todo.done,
      editing: this.todo.editing,
      editCache: ""
    };
  },

  methods: {
    removeToDo(id) {
      this.$emit("removeToDo", id);
    },
    editToDo() {
      this.editCache = this.title;
      this.editing = true;
    },
    doneEdit() {
      if (this.title.trim().length == 0) {
        this.title = this.editCache;
        alert("You cannot leave an empty task!");
      }
      this.editing = false;
      this.$emit("finishedEdit", {
        id: this.id,
        title: this.title,
        done: this.done,
        editing: this.editing
      });
    },
    cancelEdit() {
      this.title = this.editCache;
      this.editing = false;
    }
  }
};
</script>
<style lang="scss">
.todo {
  margin: 15px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 24px;
  display: grid;
  animation-duration: 0.4s;
  grid-template-columns: 1fr 10fr 1fr;
border-bottom: 1px solid #e0e0e0;
padding-bottom: 10px;

  .remove-todo {
    grid-column: 3/4;
    justify-self: center;
    color: #42b883;
  }
  .done-todo {
    justify-self: center;
    color: #42b883;
  }
  .task-info {
   
    padding-left: 15px;
  }

  .doneTask {
    text-decoration: line-through;
    opacity: 0.5;
  }
}
</style>
