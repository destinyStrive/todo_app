<template>
  <div class="todo-item" :class="{done: todoItem.done}">
    <label>
      <input
              type="checkbox"
              :checked="todoItem.done"
              @click="$emit('changeState', $event)" />
      <span class="check-button"></span>
      {{todoItem.content}}
      <!-- 设计不好的地方：删除要传事件传两层传到App组件才能实现。后续有时间的话改成Vuex实现 -->
      <span class="delete" @click="deleteClick"></span>
    </label>
  </div>
</template>

<script>
  export default {
    name: "TodoListItem",
    props: {
      todoItem: {type: Object}
    },
    methods: {
      deleteClick() {
        this.$emit('deleteEvent', this.todoItem);
      }
    }
  }
</script>

<style scoped>
  .todo-item {
    background: white;
    height: 45px;
    line-height: 45px;
    border-radius: 4px;
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
    top: 28%;
    left: 5%;
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
    margin-right: 38px;
    opacity: 0;
  }
  .todo-item input:checked + span.check-button::after {
    opacity: 1;
  }

  /* 删除按钮样式 */
  .delete {
    position: absolute;
    height: 45px;
    right: 0;
    cursor: pointer;
    border-radius: 0 4px 4px 0;

    background: linear-gradient(135deg, #f56468, #f17575);
    width: 0;
    transition: width ease-in 0.25s;
  }
  .todo-item:hover .delete {
    width: 44px;
  }
  .delete:hover {
    background: #fdc8c8;
  }
</style>