<template>
  <div class="warp">
    <div class="item" v-for="(item,index) in weekArr" :key="index"><div>{{item}}</div></div>
  </div>
</template>
<script>
import { defineComponent, computed } from "vue";
import { monthlyCalendar, getWeekNumByDateRange } from "./utils";
export default defineComponent({
  props: {
    year: Number,
    month: Number,
    startWeek: Number,
    terms: {
      type: Array,
      default: [],
    }
  },
  setup(props) {
    console.log('props: ', props.terms);

    const days = computed(() => {
      return monthlyCalendar(props.year, props.month, props.startWeek);
    });
    const weekArr = computed(()=>{
      return [
        getWeekNumByDateRange(days.value[0].date,props.terms[0].start,props.terms[0].end),
        getWeekNumByDateRange(days.value[7].date,props.terms[0].start,props.terms[0].end),
        getWeekNumByDateRange(days.value[14].date,props.terms[0].start,props.terms[0].end),
        getWeekNumByDateRange(days.value[21].date,props.terms[0].start,props.terms[0].end),
        getWeekNumByDateRange(days.value[28].date,props.terms[0].start,props.terms[0].end),
        getWeekNumByDateRange(days.value[35].date,props.terms[0].start,props.terms[0].end),
      ]
    })
    return {
      weekArr
    };
  },
});
</script>
<style lang="less" scoped>
@import "./variables.less";
.warp{
  position: absolute;
  top: @sc-week-height;
  left: 0;
  right: ~"calc(100% - 12.5%)";
  bottom: 40px;
  display: flex;
  flex-direction: column;
  .item{
    height: ~"calc(100% / 6)";
    padding: 4px;
    user-select: none;
    line-height: @sc-data-label-size;
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
  }
}
</style>
