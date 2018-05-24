<template>
    <div class="todo" v-if="!editing"   @dblclick="editToDo(todo) ">
        <input class="done-todo" type="checkbox" v-model="done" @change="doneEdit" >
        <span :class="{doneTask:done}" >{{title}}</span>
        <span class="remove-todo" @click="removeToDo(index)" > X </span>
    </div>
    <div v-else class="editing edit-todo">Editing:
        <input type="text"  class="edit-todo"
        v-model="title" @blur="doneEdit()" @keyup.enter="doneEdit()"
         @keyup.esc="cancelEdit()" v-focus>
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
    index: {
      type: Number,
      required: true
    },
    checkAll: {
      type: Boolean,
      required: true
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  watch: {
    checkAll() {
      if (this.checkAll) 
        this.done = true;
      else
        this.done = this.todo.done;
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
          index: this.index,
          todo: {
            id: this.id,
            title: this.title,
            done: this.done,
            editing: this.editing
          }
        })
      }
    ,
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
  grid-auto-columns: min-content;
  animation-duration: 0.4s;
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
