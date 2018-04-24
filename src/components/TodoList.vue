<template>
  <div class="todo-app">
    <h2>{{ msg }} <span v-if="remainingTodos > 0">Yapılacak {{remainingTodos}} iş kaldı.</span></h2>
    <input type="text" class="todo-input" placeholder="Yapılacak bir şeyler yaz..." v-model="newTodo" @keyup.enter="addTodo" ref="newTodoRef" v-focus>
    <div v-for="(todo, index) in filteredTodos" :key="index" class="todo-item">
      <input type="checkbox" class="todo-checkbox" v-model="todo.completed" v-on:change="checkTodo">
      <span v-bind:class="{ completed: todo.completed }">{{ todo.title }}</span>
      <span class="remove-item" v-on:click="removeTodo(index)">&times;</span>
    </div>
    <div v-if="todos.length > 0" class="todo-footer">
      <input type="checkbox" id="todo-checkbox-all" v-model="checkedAll" class="todo-checkbox" v-on:change="checkAllTodos">
      <label for="todo-checkbox-all">Hepsini Seç</label>
      <ul>
        <li><a href="#/all" v-bind:class="{active : listfilter == 'allList'}" v-on:click="listfilter='allList'">Tümü</a></li>
        <li><a href="#/completed" v-bind:class="{active : listfilter == 'completedlist'}" v-on:click="listfilter='completedlist'">Tamamlanan</a></li>
        <li><a href="#/remaining" v-bind:class="{active : listfilter == 'remaininglist'}" v-on:click="listfilter='remaininglist'">Kalan</a></li>
      </ul>
      <input type="button" v-if="completedTodos > 0 && listfilter!='remaininglist'" class="clear-completed" value="Temizle" v-on:click="clearCompleted">
    </div>
  </div>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      msg: "Merhaba!",
      newTodo: "",
      listfilter: "allList",
      todos: [
        {
          id: 1,
          title: "Ekmek almaya git.",
          completed: false
        },
        {
          id: 2,
          title: "Bulaşıkları yıka.",
          completed: false
        },
        {
          id: 3,
          title: "Akrabalarını ara.",
          completed: false
        }
      ],
      checkedAll: false
    };
  },
  directives: {
    focus: {
      // directive definition
      inserted: function(el) {
        el.focus();
      }
    }
  },
  computed: {
    remainingTodos() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    completedTodos() {
      return this.todos.filter(todo => todo.completed).length;
    },
    filteredTodos() {
      if (this.listfilter == "completedlist")
        return this.todos.filter(todo => todo.completed);
      else if (this.listfilter == "remaininglist")
        return this.todos.filter(todo => !todo.completed);
      else return this.todos;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.length > 0) {
        this.todos.push({
          id: this.todos.length == 0 ? 1 : this.todos.slice(-1)[0].id + 1,
          title: this.newTodo,
          completed: false
        });
        this.newTodo = "";
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
      this.$refs.newTodoRef.focus();
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
      this.checkedAll = false;
      this.$refs.newTodoRef.focus();
    },
    checkAllTodos() {
      this.todos.forEach(todo => {
        todo.completed = event.target.checked;
      });
    },
    checkTodo() {
      this.todos.some(todo => {
        if (!todo.completed) {
          this.checkedAll = false;
          return true;
        }
        this.checkedAll = true;
      });
    }
  }
};
</script>

<style lang="scss">
@import "./../css/variables";
.todo-app {
  font-size: $primary-font-size;
  .todo-input {
    width: 100%;
    padding: 10px;
    margin-bottom: 25px;
  }
  .todo-item {
    margin-bottom: 25px;
    border-bottom: 1px solid #c8c8c8;
    line-height: 35px;
    &:hover {
      .remove-item {
        display: block;
      }
    }
    .remove-item {
      float: right;
      font-weight: bold;
      font-size: 28px;
      cursor: pointer;
      display: none;
    }
  }
  .todo-checkbox {
    width: 18px;
    height: 18px;
    cursor: pointer;
  }
  label {
    font-size: $secondary-font-size;
  }
  .completed {
    text-decoration: line-through;
    color: gray;
  }
  .clear-completed {
    float: right;
    border: none;
    outline: none;
    background: none;
    color: $secondary-color;
    padding: 10px 15px;
    margin: 13px 0;
    text-align: center;
    text-decoration: none;
    font-size: $secondary-font-size;
    cursor: pointer;
    &:hover {
      text-decoration: underline;
    }
  }
  label[for="todo-checkbox-all"] {
    position: relative;
    bottom: 3px;
  }
  ul {
    list-style-type: none;
    display: inline-block;
    font-size: $secondary-font-size;
    li {
      display: inherit;
      margin: 5px;
      a {
        text-decoration: none;
        color: $primary-color;
        padding: 5px;
        border: 1px solid transparent;
        &:hover {
          border: 1px solid gray;
        }
      }
    }
  }
  .active {
    border: 1px solid gray;
  }
}
</style>
