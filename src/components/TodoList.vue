<template>
  <form @submit.prevent="addTodo" class="header">
    <h1>Vue3 - Todo App</h1>
    <input type="text" v-model="todo" name="todo" placeholder="Add new todo" />
    <button class="add-btn" type="submit">Add</button>
  </form>

  <div class="todo-list-header">
    <h1>Todo List</h1>
    <div v-if="todos.length" class="options">
      <!-- <button @click="markAllCompleted">Mark All Completed</button> -->
      <!-- <button @click="markAllIncompleted">Mark All Incompleted</button> -->
      <button @click="removeAllTodos">Remove All</button>
      <button @click="removeAllCompletedTodos">
        Remove All Completed Todos
      </button>
      <button @click="removeAllIncompletedTodos">
        Remove All Incompleted Todos
      </button>
    </div>
  </div>

  <div v-if="todos.length">
    <ul>
      <li v-for="(todo, index) in todos" :key="todo.id">
        <div
          class="todo"
          :class="{ completed: todo.completed }"
          @dblclick="toggleCompleted(todo)"
        >
          {{ todo.content }}

          <span @click="removeTodo(index)" class="close">&#10005;</span>
        </div>
      </li>
    </ul>
  </div>
  <div v-else class="no-todo-present">No Todos</div>
</template>

<script>
export default {
  name: "todos",
  data() {
    return {
      todo: "",
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    addTodo() {
      if (this.todo === "") return;

      this.todos.push({
        id: Date.now(),
        completed: false,
        content: this.todo,
      });

      this.todo = "";
      this.saveData();
    },

    toggleCompleted(todo) {
      todo.completed = !todo.completed;
      this.saveData();
    },

    removeTodo(id) {
      this.todos.splice(id, 1);
      this.saveData();
    },

    // markAllCompleted() {
    //   this.todos.forEach((todo) => (todo.completed = true));
    //   this.saveData();
    // },

    // markAllIncompleted() {
    //   this.todos.forEach((todo) => (todo.completed = false));
    //   this.saveData();
    // },

    removeAllTodos() {
      this.todos = [];
      this.saveData();
    },

    removeAllCompletedTodos() {
      this.todos = this.todos.filter((todo) => todo.completed === !true);
      this.saveData();
    },

    removeAllIncompletedTodos() {
      this.todos = this.todos.filter((todo) => todo.completed === !false);
      this.saveData();
    },

    saveData() {
      const storageData = JSON.stringify(this.todos);
      localStorage.setItem("todos", storageData);
    },
  },
};
</script>

<style scoped>
.header {
  background-color: #41b883;
  color: #fff;
  padding: 30px 40px;
  text-align: center;
}

.header:after {
  content: "";
  display: table;
  clear: both;
}
.header h1 {
  margin-bottom: 15px;
}

input {
  margin: 0;
  border: none;
  border-radius: 0;
  width: 75%;
  padding: 10px;
  float: left;
  font-size: 16px;
}

.add-btn {
  width: 25%;
  background: #d9d9d9;
  color: #555;
  padding: 10px;
  text-align: center;
  font-weight: bold;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
  border-radius: 0;
  border: 0;
}

.add-btn:hover {
  background-color: #bbb;
}

.todo-list-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #333859;
  color: #fff;
  padding: 15px 40px;
}
.todo-list-header .options {
  display: flex;
  overflow-x: auto;
  margin-left: 10px;
}

.todo-list-header .options button {
  background: #41b883;
  color: #fff;
  border: 1px solid #fff;
  padding: 5px;
  margin-right: 10px;
}

.no-todo-present {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
  font-size: 35px;
}

ul {
  margin: 0;
  padding: 0;
}

ul li {
  cursor: pointer;
  position: relative;
  background: #eee;
  transition: 0.2s;
  list-style-type: none;
  user-select: none;
}

ul li:nth-child(odd) {
  background: #f9f9f9;
}

ul li:hover {
  background: #ddd;
}

.todo {
  cursor: pointer;
  padding: 12px 8px 12px 40px;
}

.completed {
  background: #888;
  color: #fff;
  text-decoration: line-through;
}

.completed::before {
  content: "";
  position: absolute;
  border-color: #fff;
  border-style: solid;
  border-width: 0 2px 2px 0;
  top: 10px;
  left: 16px;
  transform: rotate(45deg);
  height: 15px;
  width: 7px;
}

.close {
  position: absolute;
  right: 0;
  top: 0;
  padding: 12px 16px 12px 16px;
}

.close:hover {
  background-color: #f44336;
  color: white;
}
</style>
