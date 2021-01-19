<template>
  <div class="filters">
        <span class="filter"
              v-for="filter in filtersText"
              :key="filter"
              :class="{active: selected == filter}"
              @click="$emit('filter-change-event', filter)"
        >
          {{filter}}
        </span>
  </div>
</template>

<script>
  export default {
    name: "TodoFilter",
    props: ["selected"], // 数组形式的props。也可以是对象形式
    data() {
      return {
        filtersText: ['All', 'Finished', 'Unfinished'],
      }
    },
  }
</script>

<style scoped>
  .filters {
    margin-top: 25px; /* ==这个样式暂时在组件里面写定== */
  }
  .filters .filter {
    padding: 5px;
    font-size: 13px;
    color: #878484;
    cursor: pointer;
  }
  .filters .filter.active {
    color: #6b729c;
    font-size: 17px;
  }
</style>