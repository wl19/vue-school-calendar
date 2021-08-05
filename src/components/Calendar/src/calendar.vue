<template>
  <div class="vue-schedule-calendar">
    <header-bar
      :year="year"
      :month="month"
      @updateValue="updateView"
      :title="title"
    ></header-bar>
    <div class="vue-schedule-calendar-body">
      <week :startWeek="startWeek"></week>
      <weekNum :year="year" :month="month" :startWeek="startWeek" :terms="terms" />
      <month
        :year="year"
        :month="month"
        :startWeek="startWeek"
        :data="events"
        :colorList="colorList"
      ></month>
      <eventList :events="events" :colorList="colorList" />
    </div>
  </div>
</template>
<script>
import { defineComponent, reactive, toRefs } from "vue";
import headerBar from "./header.vue";
import week from "./week.vue";
import month from "./month.vue";
import eventList from './eventList.vue'
import weekNum from './weekNum.vue'
export default defineComponent({
  name: "schedule-calendar",
  components: {
    headerBar,
    week,
    month,
    eventList,
    weekNum
  },
  props: {
    startMonth: String,
    startWeek: {
      type: Number,
      default: 1,
    },
    events: {
      type: Array,
      default: [],
    },
    colorList: {
      type: Array,
      default: ["#45b97c","#ef4136","#2196f3"],
    },
    terms: {
      type: Array,
      default: [],
    },
    title: String
  },
  setup() {
    const state = reactive({
      year: new Date().getFullYear(),
      month: new Date().getMonth(),
    });

    const updateView = (year, month) => {
      state.year = year;
      state.month = month;
    };
    return {
      ...toRefs(state),
      updateView,
    };
  },
});
</script>
<style lang="less">
@import "./variables.less";

.vue-schedule- {
  &calendar {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    min-width: @sc-cell-min-width * 7;
    color: @sc-base-color;
    font-size: @sc-base-font-size;
    box-shadow: @sc-box-shadow;

    *,
    *::before,
    *::after {
      box-sizing: border-box;
    }

    button {
      border: 0;
      outline: none;
      cursor: pointer;
      background: transparent;
    }

    &-body {
      position: relative;
      flex: 1;
      width: 100%;
      overflow: hidden;
      background: @sc-body-color;
    }
  }
}
</style>
