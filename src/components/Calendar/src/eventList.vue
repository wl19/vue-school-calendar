<template>
  <div class="warp">
    <div v-for="(item, index) in events" :key="item.start" class="list">
      <div
        class="dot"
        :style="{
          background: item.background ? item.background : colorList[index],
        }"
      ></div>
      <div class="text">{{ item.title }}</div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
export default defineComponent({
  props: {
    events: {
      type: Array,
      default: [],
    },
    colorList: {
      type: Array,
      default: [],
    },
  },
});
</script>
<style lang="less" scoped>
.warp {
  display: flex;
  justify-content: center;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  .list {
    display: flex;
    margin: 20px;
    .dot {
      width: 20px;
      height: 20px;
      display: inline-block;
      border-radius: 5px;
      margin-right: 5px;
    }
    .text {
      line-height: 20px;
    }
  }
}
</style>
