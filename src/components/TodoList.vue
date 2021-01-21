<template>
  <div class="todo-item-container">
    <todo-list-item
            v-for="item in toDoList"
            :key="item.id"
            :todo-item="item"
            @change-state="item.done = $event.target.checked"
            @delete-event="$emit('deleteEvent', $event)"
    >
    </todo-list-item>
  </div>
</template>

<script>
  import TodoListItem from "./TodoListItem";
  export default {
    name: "TodoList",
    components: {TodoListItem},
    props: {
      toDoList: {type: Array}
    },
  }
</script>

<style scoped>
  .todo-item-container {
    display: grid;
    row-gap: 14px;
    margin-top: 25px; /* ==这个样式暂时在组件里面写定== */
  }
</style>