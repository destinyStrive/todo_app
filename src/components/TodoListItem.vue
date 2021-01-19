<template>
  <div class="todo-item" :class="{done: todoItem.done}">
    <label>
      <input
              type="checkbox"
              :checked="todoItem.done"
              @click="$emit('changeState', $event)" />
      <span class="check-button"></span>
      {{todoItem.content}}
    </label>
  </div>
</template>

<script>
  export default {
    name: "TodoListItem",
    props: {
      todoItem: {type: Object}
    }
  }
</script>

<style scoped>
  .todo-item {
    background: white;
    padding: 16px;
    border-radius: 8px;
    color: #626262;
  }
  .todo-item label {
    position: relative;
    display: flex;
    align-items: center;
    cursor: pointer;
  }
  .todo-item.done label {
    font-style: italic;
    text-decoration: line-through;
  }
  .todo-item label span.check-button {
    position: absolute;
    top: -1px;
  }
  .todo-item label span.check-button::before,
  .todo-item label span.check-button::after {
    content: "";
    display: block;
    position: absolute;
    width: 18px;
    height: 18px;
    border-radius: 50%;
  }
  .todo-item label span.check-button::before {
    border: 1px solid #b382f9;
  }
  .todo-item label span.check-button::after {
    transition: 0.4s;
    background: #b382f9;
    transform: translate(1px, 1px) scale(0.7);
    opacity: 0;
  }
  .todo-item input {
    margin-right: 16px;
    opacity: 0;
  }
  .todo-item input:checked + span.check-button::after {
    opacity: 1;
  }
</style>