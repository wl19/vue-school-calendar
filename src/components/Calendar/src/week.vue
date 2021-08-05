<template>
  <div class="schedule-calendar-week">
    <span v-for="wk in weeks" :key="wk" class="schedule-calendar-week-item">{{
      wk
    }}</span>
  </div>
</template>
<script>
import { defineComponent, reactive, computed } from "vue";
export default defineComponent({
  props: {
    startWeek: Number,
  },
  setup(props) {
    const state = reactive({
      cn: ["日", "一", "二", "三", "四", "五", "六"],
      en: [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ],
    });

    const weeks = computed(() => {
      return props.startWeek === 0
        ? state.cn
        : state.cn.concat(state.cn.splice(0, props.startWeek));
    });

    return {
      weeks,
    };
  },
});
</script>

<style lang="less" scoped>
@import "./variables.less";

.schedule-calendar- {
  &week {
    display: flex;
    height: @sc-week-height;
    line-height: @sc-week-height;
    background: @sc-primary-color;
  }
  &week-item {
    flex: 1;
    text-align: center;
    color: @sc-body-color;
  }
}
</style>
