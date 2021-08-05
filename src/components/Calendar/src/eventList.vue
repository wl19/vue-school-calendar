<template>
<div class="warp">
  <div class="list">
    <div class="dot" :style="{background: '#1976d2'}"></div>
    <div class="text">今天</div>
  </div>
  <div v-for="item in events" :key="item.id"  class="list">
    <div class="dot" :style="{background: item.background}"></div>
    <div class="text">{{item.text}}</div>
  </div>
</div>
</template>

<script>
import { defineComponent } from 'vue'
export default defineComponent({
  props: {
    events: {
      type: Array,
      default: [],
    }
  }
})
</script>
<style lang="less" scoped>
.warp{
  display: flex;
  justify-content: center;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  .list {
    display: flex;
    margin: 20px;
    .dot{
      width: 20px;
      height: 20px;
      display: inline-block;
      border-radius: 5px;
      margin-right: 5px;
    }
    .text{
      line-height: 20px;
    }
  }
}

</style>