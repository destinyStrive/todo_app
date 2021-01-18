<template>
  <div id="app">
    <div class="container">
      <h2 class="todo-title">欢迎使用Destiny待办事项！</h2>
      
      <div class="add-item">
        <input type="text" class="add-item-input" v-model="new_todo" />
        <button @click="addItem">
          <i class="add-item-button">+</i>
        </button>
      </div>
      
      <div class="filters">
        <span class="filter">全部</span>
        <span class="filter">已完成</span>
        <span class="filter">未完成</span>
      </div>

      <ul class="todo-item-container">
        <li v-for="item in toDoList" :key="item.content">
          <input type="checkbox">
          <span>{{item.content}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    components: {

    },
    data() {
      return {
        toDoList: [
          {'id': 0, 'content': 'item 1'},
          {'id': 1, 'content': 'item 2'}
        ],
        new_todo: ''
      }
    },
    methods: {
      addItem() {
        if(this.new_todo != '') {
           this.toDoList.unshift({
             'id': this.toDoList.length,
             'content': this.new_todo
           });
        }
      },
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body {
    background-color: #7f88b1;
  }
  .container{
    width: 20vw;
    height: 500px;
    border: none;
    border-radius: 30px;
    background-color: #e2e5ea;
    box-shadow: 0 0 100px #abaebd;
    margin: 100px auto;
    padding: 35px;
    display: grid;
  }
  .todo-title {
    color: #485072;
    text-align: center;
  }

  .add-item {
    position: relative;
    outline: none;
  }
  .add-item-input {
    font-size: 30px;
    width: 100%;
    height: 40px;
    border-radius: 40px;
    border: none;
    outline: none; /* 当鼠标点击input框的时候，外沿会有黑色边框
                      outline: none可以去掉它
                    */
  }
  .add-item button {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: linear-gradient(#c0a5f3, #7f95f7);

    border: none;
    outline: none;

    color: white;
    position: absolute;
    right: 0;

    cursor: pointer;
  }
  .add-item-button {
    font-size: 20px;
  }

  .todo-item-container {
    background: yellow;
  }
  li {
    list-style: none;
  }
</style>
