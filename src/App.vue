<template>
  <div class="container">
    <header id="main-header" class="bg-success text-white p-4 mb-3">
      <div class="container">
        <div class="row">
          <div class="col-md-6">
            <h1 id="header-title">Todo App</h1>
          </div>
          <div class="col-md-3 align-self-center">
            <input
              type="text"
              class="form-control"
              style="width:200%"
              id="filter-todos"
              placeholder="Search todos"
            />
          </div>
        </div>
      </div>
    </header>
    <div id="main" class="card card-body">
      <form
        class="form-inline mb-3"
        id="create-todo"
        action
        method="post"
        @submit.prevent="addTodo"
      >
        <input
          id="todo-name"
          v-model="todoName"
          type="text"
          placeholder="Add Todo"
          class="form-control mr-5"
        />
        <input id="todo-duedate" v-model="todoDueDate" type="date" class="form-control mr-5" />
        <button class="btn btn-dark float-right" @click="addTodo">Submit</button>
      </form>
      <div class="container">
        <button class="btn-dark float-left" id="control-complete" value="false">Hide Completed</button>

        <select name="sort-options" id="select-sort" class="float-right">
          <option value="null">--</option>
          <option value="Sort Date">Sort Date</option>
          <option value="Sort Name">Sort Name</option>
        </select>
      </div>
      <br />
      <ul id="todos">
        <li v-for="(todo,index) in todosArray" :key="index">
          <span contenteditable="true" class="editable" id="edit-name">{{todo.todoName}}</span>
          <button class='btn btn-danger btn-sm float-right delete'
           >🗑️</button>
          <span id="todo-duedate-li" class="float-right">{{todo.todoDueDate}}</span>
          <input type="date" id="edit-date" class="float-right">
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      todoName: "",
      todoDueDate: "",
      todoStatus: false,
      todosArray: []
    };
  },
  methods: {
    addTodo() {
      let todoName = this.todoName;
      let todoDueDate = this.todoDueDate;
      let todoStatus = this.todoStatus;
      this.todosArray.push({ todoName, todoDueDate, todoStatus });
    }
  }
};
</script>

<style>
</style>