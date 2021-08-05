<template>
  <div class="warp">
    <div class="title">{{title}}</div>
    <header class="schedule-calendar-hd">
    <button
      type="button"
      class="schedule-calendar-arrow double-arrow"
      @click="prevYear"
    >
      &lt;&lt;
    </button>
    <button type="button" class="schedule-calendar-arrow" @click="prevMonth">
      &lt;
    </button>
    <div class="schedule-calendar-picker" ref="picker">
      <div
        role="button"
        class="schedule-calendar-display"
        @click="pickerVisible = !pickerVisible"
      >
        {{ year }} 年 {{ month + 1 }} 月
      </div>
      <Picker
        :visible="pickerVisible"
        :year="year"
        :month="month"
        @selectYear="selectYear"
        @selectMonth="selectMonth"
      ></Picker>
    </div>
    <button type="button" class="schedule-calendar-arrow" @click="nextMonth">
      &gt;
    </button>
    <button
      type="button"
      class="schedule-calendar-arrow double-arrow"
      @click="nextYear"
    >
      &gt;&gt;
    </button>
  </header>
  </div>
  
</template>
<script>
import {
  defineComponent,
  reactive,
  ref,
  toRefs,
  onBeforeMount,
  onUnmounted,
} from "vue";
import { calcPrevMonth, calcNextMonth } from "./utils";
import Picker from "./picker.vue";

export default defineComponent({
  components: {
    Picker,
  },
  props: {
    year: Number,
    month: Number,
    title: String
  },
  emits: ["updateValue"],
  setup(props, { emit }) {
    const state = reactive({
      pickerVisible: false,
    });
    const picker = ref(null);

    const updateValue = (year, month = props.month) => {
      emit("updateValue", year, month);
    };

    const prevYear = () => {
      updateValue(props.year - 1);
    };

    const nextYear = () => {
      updateValue(props.year + 1);
    };

    const prevMonth = () => {
      const { year, month } = calcPrevMonth(props.year, props.month);
      updateValue(year, month);
    };

    const nextMonth = () => {
      const { year, month } = calcNextMonth(props.year, props.month);
      updateValue(year, month);
    };

    const clickOutSide = (e) => {
      if (state.pickerVisible && !picker.value.contains(e.target)) {
        state.pickerVisible = false;
      }
    };

    onBeforeMount(() => {
      document.addEventListener("mouseup", clickOutSide);
    });
    onUnmounted(() => {
      document.removeEventListener("mouseup", clickOutSide);
    });

    const selectYear = (year) => {
      updateValue(year);
    };

    const selectMonth = (month) => {
      updateValue(props.year, month);
    };

    return {
      ...toRefs(state),
      picker,
      prevYear,
      nextYear,
      prevMonth,
      nextMonth,
      selectYear,
      selectMonth,
    };
  },
});
</script>
<style lang="less" scoped>
@import "./variables.less";

.warp{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  background: @sc-primary-color;

  .title{
    color: #fff;
  }
}

.schedule-calendar- {
  &hd {
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
    height: @sc-header-height;
    padding: @sc-header-padding 0;
    font-size: @sc-header-fs;
    line-height: @sc-header-height - @sc-header-padding * 2;
    
    color: @sc-body-color;
    user-select: none;
  }
  &arrow {
    font-family: consolas;
    font-size: inherit;
    font-weight: 400;
    padding: 0 10px;
    height: 100%;
    color: @sc-primary-light-color;

    &:active {
      background: darken(@sc-primary-dark-color, 15%);
    }
    &.double-arrow {
      letter-spacing: -3px;
    }
  }
  &picker {
    position: relative;
    z-index: 20;
    padding: 4px 5px;
    height: 100%;
  }
  &arrow,
  &display {
    border-radius: 2px;
    transition: 0.2s ease-in-out;
    &:hover {
      color: #fff;
      background: @sc-primary-dark-color;
    }
  }
  &display {
    padding: 0 10px;
    line-height: 32px;
    cursor: pointer;
  }
  
}
</style>
