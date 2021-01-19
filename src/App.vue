<template>
  <div id="app">
    <div class="container">
      <h2 class="todo-title">Welcome to your todo-app!</h2>
      <todo-add @add-event="addTodo" />
      <!-- ****以下todo-filter的参数写法，我把它理解为一种思想：****
           1. 在父子组件通信的时候，父组件先将一个子组件要用的值（比如这里filter组件要用showType）传给子组件的props
           2. 然后子组件再通过自定义事件（比如这里的filter-change-event）来修改父组件中这个showType的值
       -->
      <todo-filter :selected="showType" @filter-change-event="showType = $event" />

      <div v-if="toDoList.length > 0">
        <todo-list :to-do-list="filteredTodo" />
        <div class="percentage" :style="{display: showComplete ? 'block' : 'none'}">
          <p>Completed tasks: {{completedPercentage}}</p>
          <hr/>
        </div>
        <div class="control-buttons">
          <span @click="showComplete = !showComplete">
            <span v-if="!showComplete">Show Complete</span>
            <span v-else>Hide Complete</span>
          </span>
          <span @click="clearAll">Clear All</span>
        </div>
      </div>
      <!-- 没有todo显示一段文字 -->
      <div v-else class="noTodoContainer">
        <img src="./assets/celebration.jpg" alt="celebration" style="width: 30px; height: 30px;">&nbsp;&nbsp;&nbsp;
        <h4>No todos left. Time to chill !</h4>
      </div>
    </div>

  </div>
</template>

<script>
  import TodoAdd from "./components/TodoAdd";
  import TodoFilter from "./components/TodoFilter";
  import TodoList from "./components/TodoList";
  export default {
    name: 'App',
    components: {TodoList, TodoFilter, TodoAdd},
    data() {
      return {
        toDoList: [
          {id: '0', content: 'ToDo1', done: false},
          {id: '1', content: 'ToDo2', done: false},
          {id: '2', content: 'ToDo3', done: false},
        ],
        showType: 'All', // 控制子组件todo-list要显示的类型
        showComplete: false,
      }
    },
    methods: {
      addTodo(content) {
        this.toDoList.push({id: this.toDoList.length, content: content, done: false});
      },
      clearAll() {
        this.toDoList = [];
      }
    },
    computed: {
      filteredTodo() { // 根据showType, 返回不同的数组
        switch (this.showType) {
          case 'All':
            return this.toDoList;
          case 'Finished':
            return this.toDoList.filter((todo) => todo.done);
          default:
            return this.toDoList.filter((todo) => !todo.done);
        }
      },
      completedPercentage() {
        let percentage = this.toDoList.filter((todo) => todo.done).length / this.toDoList.length;
        return (percentage * 100).toFixed(2) + '%';
      }
    }
  }
</script>

<style>
  /* 全局样式 */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Helvetica;
  }
  body {
    background-color: rgb(203, 210, 240);
  }
  /* 外层容器样式 */
  .container{
    width: 20vw;
    min-height: 50vh;
    border: none;
    border-radius: 30px;
    background-color: #e2e5ea;
    box-shadow: 0 0 24px rgba(0, 0, 0, 0.15);
    margin: 100px auto;
    padding: 35px;
  }
  /* 标题样式 */
  .todo-title {
    color: #485072;
    text-align: center;
  }
  /* 显示进度的p标签样式 */
  .percentage {
    margin-top: 30px;
  }
  /* 底部控制按钮样式 */
  .control-buttons {
    margin-top: 25px;
    text-align: center;
  }
  .control-buttons span {
    font-family: 'Open Sans', sans-serif;
    font-weight: bold;
    margin-right: 20px;
    cursor: pointer;
  }
  /* 没有任务的div样式 */
  .noTodoContainer {
    height: 200px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
