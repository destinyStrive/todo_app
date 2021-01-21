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
        <todo-list :to-do-list="filteredTodo" @delete-event="deleteItem" />
        <div class="percentage" :style="{display: showComplete ? 'block' : 'none'}">
          <p>Completed tasks: {{completedPercentage}}</p>
          <hr/>
        </div>
        <div class="control-buttons">
          <span @click="showComplete = !showComplete" class="control-button">
            <span v-if="!showComplete">Show Complete</span>
            <span v-else>Hide Complete</span>
          </span>
          &nbsp;&nbsp;&nbsp;&nbsp;
          <span @click="clearAll" class="control-button">Clear All</span>
        </div>
      </div>
      <!-- 没有todo显示一段文字 -->
      <transition name="slide-fade">
        <p class="status free" v-if="toDoList.length == 0">
          <img src="./assets/beer_celebration.svg" alt="celebration">
          No todos left. Time to chill!
        </p>
      </transition>
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
      deleteItem(item) {
        const idx = this.toDoList.indexOf(item);
        this.toDoList.splice(idx, 1);
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
    min-height: 58vh;
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
  /* 没有任务的div样式 */
  .status.free {
    font-weight: bold;
    text-align: center;
    margin-top: 75px;
  }
  .status.free img {
    display: block;
    width: 50px;
    margin: 10px auto;
  }
  /* transition的class有6种切换
    1. enter-from/enter-active/enter-to
        enter-from: 元素被插入之前生效，可以看做是进场动画。进场之后，动画移除
    2. leave-from/leave-active/leave-to
        leave-to: 可以看做是出场动画
    v-enter-active 和 v-leave-active 可以控制进入/离开过渡的不同的缓和曲线，
  */
  .slide-fade-enter-active, .slide-fade-leave-active {
    transition: all 0.3s ease;
  }
  .slide-fade-enter-from, .slide-fade-leave-to {
    transform: scale(1.1);
    opacity: 0;
  }
  /* 底部控制按钮样式 */
  .control-buttons {
    margin-top: 25px;
    text-align: center;
  }
  .control-buttons span {
    font-family: 'Open Sans', sans-serif;
    font-weight: bold;
    /*margin-right: 20px;*/
    cursor: pointer;

    position: relative;
  }
  /* hover到按钮上时，下划线的动画效果 */
  .control-button::before {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;

    height: 2px;
    width: 0;
    background: #486bee;

    transition: width 0.2s linear;
  }
  .control-button:hover::before {
    width: 100%;
  }
</style>
