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
              v-model="findTodoName"
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
        <button class="btn btn-dark float-right" @click.prevent="addTodo">Submit</button>
      </form>
      <div class="container">
        <button
          class="btn-dark float-left"
          id="control-complete"
          value="false"
          @click.prevent="hideCompleted($event)"
        >Hide Completed</button>

        <select name="sort-options" id="select-sort" class="float-right" @input="todoSort($event)">
          <option value="null">--</option>
          <option value="Sort Date">Sort Date</option>
          <option value="Sort Name">Sort Name</option>
        </select>
      </div>
      <br />
      <div class="container">
        <b-list-group id="todos" v-for="(todo,index) in filterTodos" :key="index">
          <TodoItem
            :todo="todo"
            :index="index"
            :completeTodo="completeTodo"
            :editTodoName="editTodoName"
            :deleteTodo="deleteTodo"
            :editTodoDueDate="editTodoDueDate"
          />
        </b-list-group>
      </div>
    </div>
  </div>
</template>
<script>
import TodoItem from "./components/TodoItem.vue";
export default {
  name: "app",
  data() {
    return {
      todoName: "",
      todoDueDate: "",
      todoStatus: false,
      todosArray: [],
      findTodoName: "",
      hideCompletedFlag: false
    };
  },
  components: {
    TodoItem
  },
  computed: {
    filterTodos() {
      let text = new RegExp(this.findTodoName, "i");
      if (!this.hideCompletedFlag)
        return this.todosArray.filter(todo => todo.todoName.match(text));
      else
        return this.todosArray.filter(
          todo => todo.todoName.match(text) && todo.todoStatus === false);
    },
    getTodoID(){
        return this.todosArray.length;
    }
  },
  mounted() {
    if (JSON.parse(localStorage.getItem("Todos")) != null){
      this.todosArray = JSON.parse(localStorage.getItem("Todos"));
    }
  },
  methods: {
    addTodo() {
      let todoName = this.todoName;
      let todoDueDate = this.todoDueDate
        .split("-")
        .reverse()
        .join("-");
      let todoStatus = this.todoStatus;
      let todoID = this.getTodoID;
      this.todosArray.push({ todoID, todoName, todoDueDate, todoStatus });
      this.todoName = "";
      this.todoDueDate = "";
      localStorage.setItem("Todos", JSON.stringify(this.todosArray));
    },
    deleteTodo(e, index) {
      const ID = this.filterTodos[index].todoID;
      const todosArrayIndex = this.todosArray.indexOf(
        this.todosArray.find(item => item.todoID === ID)
      );
      this.todosArray.splice(todosArrayIndex, 1);
      this.todosArray.forEach((todo,index)=>todo.todoID=index)
      localStorage.setItem("Todos", JSON.stringify(this.todosArray));
    },
    editTodoName(e, index) {
      e.preventDefault();
      this.filterTodos[index].todoName = e.target.innerText;
      e.target.blur();
      localStorage.setItem("Todos", JSON.stringify(this.todosArray));
    },
    completeTodo(e,index) {
      e.target.checked=this.filterTodos[index].todoStatus;
      this.filterTodos[index].todoStatus =
        this.filterTodos[index].todoStatus !== true;
      localStorage.setItem("Todos", JSON.stringify(this.todosArray));
    },
    editTodoDueDate(e, index) {
      this.filterTodos[index].todoDueDate = e.target.value
        .split("-")
        .reverse()
        .join("-");
      e.target.value=null
      localStorage.setItem("Todos", JSON.stringify(this.todosArray));
    },
    hideCompleted(e) {
      if (
        this.hideCompletedFlag === false &&
        this.todosArray.find(item => item.todoStatus === true)
      ) {
        e.target.innerText = "Show All";
        this.hideCompletedFlag = true;
      } else {
        e.target.innerText = "Hide Completed";
        this.hideCompletedFlag = false;
      }
    },
    todoSort(e) {
      if (e.target.value === "Sort Name") {
        this.todosArray.sort((a, b) => a.todoName.localeCompare(b.todoName));
      } else if (e.target.value === "Sort Date") {
        this.todosArray.sort((a, b) =>
          a.todoDueDate
            .split("-")
            .reverse()
            .join("-") >
          b.todoDueDate
            .split("-")
            .reverse()
            .join("-")
            ? 1
            : a.todoDueDate
                .split("-")
                .reverse()
                .join("-") <
              b.todoDueDate
                .split("-")
                .reverse()
                .join("-")
            ? -1
            : 0
        );
      }
    }
  }
};
</script>

<style>
</style>