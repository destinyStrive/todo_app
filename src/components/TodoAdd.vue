<template>
  <div class="add-item">
    <input type="text"
           class="add-item-input"
           v-model="new_todo"
           @keyup.enter="addItem"
    />
    <button @click="addItem">
      <i class="add-item-button">+</i>
    </button>
  </div>
</template>

<script>
  export default {
    name: "TodoAdd",
    data() {
      return {
        new_todo: '',
      }
    },
    methods: {
      addItem() {
        // 通过自定义事件实现子组件向父组件传递数据
        if(this.new_todo != ''){
          this.$emit('addEvent', this.new_todo);
        }
      }
    }
  }
</script>

<style scoped>
  .add-item {
    position: relative;
    outline: none;
    margin-top: 25px; /* ==这个样式暂时在组件里面写定== */
  }
  .add-item-input {
    padding: 16px 52px 16px 18px; /* 设置输入框的内边距，这样可以让输入的文字和外沿有一定的空白 */
    font-size: 15px;
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

    transition: transform 0.2s ease;
  }
  .add-item button:hover {
    transform: scale(1.2);
  }
  .add-item-button {
    font-size: 20px;
  }
</style>