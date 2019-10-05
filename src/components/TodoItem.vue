<template>
  <div id="todo-item">
    <b-list-group-item>
      <input
        id="complete"
        type="checkbox"
        class="float-left"
        :checked="todo.todoStatus"
        @change.prevent="completeTodo($event,index)"
      />
      <span
        contenteditable="true"
        class="editable"
        id="edit-name"
        @keydown.tab="editTodoName($event,index)"
        @keydown.enter="editTodoName($event,index)"
      >{{todo.todoName}}</span>
      <b-button class="btn btn-danger btn-sm float-right delete" @click="deleteTodo($event,index)">🗑️</b-button>
      <span id="todo-duedate-li" class="float-right">{{todo.todoDueDate}}</span>
      <input type="date" id="edit-date" class="float-right" @keyup.enter="editTodoDueDate($event,index)" />
    </b-list-group-item>
  </div>
</template>

<script>
export default {
  name: "todo-item",
  props: {
    todo: Object,
    editTodoName: Function,
    deleteTodo: Function,
    editTodoDueDate: Function,
    completeTodo: Function,
    index: Number
  }
};
</script>

<style>
</style>