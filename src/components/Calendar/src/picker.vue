<template>
  <div class="schedule-calendar-picker-list" v-if="visible">
    <div class="schedule-calendar-picker-col" style="width: 60%">
      <ul ref="yearList">
        <li
          v-for="n in endYear - beginYear"
          :key="n"
          :class="{ active: beginYear + n === year }"
          @click="selectYear(beginYear + n)"
        >
          {{ beginYear + n }}
        </li>
      </ul>
    </div>
    <div class="schedule-calendar-picker-col" style="width: 40%">
      <ul ref="monthList">
        <li
          v-for="n in 12"
          :key="n"
          :class="{ active: n === month + 1 }"
          @click="selectMonth(n - 1)"
        >
          {{ n }}
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
import { defineComponent, watch, nextTick, ref } from "vue";
export default defineComponent({
  props: {
    beginYear: {
      type: Number,
      default: 1900,
    },
    endYear: {
      type: Number,
      default: 2100,
    },
    year: Number,
    month: Number,
    visible: Boolean,
  },
  emits: ["selectYear", "selectMonth"],
  setup(props, { emit }) {
    const yearList = ref(null);
    const monthList = ref(null);

    watch(
      () => props.visible,
      (val) => {
        if (!val) return;
        nextTick(() => {
          yearList.value.querySelector("li.active").scrollIntoView();
          monthList.value.querySelector("li.active").scrollIntoView();
        });
      }
    );

    const selectYear = (year) => {
      emit("selectYear", year);
    };
    const selectMonth = (month) => {
      emit("selectMonth", month);
    };

    return {
      yearList,
      monthList,
      selectYear,
      selectMonth,
    };
  },
});
</script>
<style lang="less" scoped>
@import "./variables.less";

.schedule-calendar-picker- {
  &list {
    position: absolute;
    top: 100%;
    left: 0;
    display: flex;
    width: 100%;
    font-size: @sc-base-font-size;
    color: @sc-gray-color;
    background: @sc-body-color;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
  }
  &col {
    overflow: hidden;
    height: @sc-picker-height;
    padding-bottom: 100%;
    &:first-child {
      border-right: 1px solid @sc-border-color;
    }
    ul,
    li {
      list-style: none;
    }
    ul {
      padding: 0;
      margin: 0;
    }
    li {
      padding: 0 12px;
      line-height: 30px;
      cursor: pointer;
      &:hover {
        background: lighten(@sc-primary-light-color, 8%);
      }
      &.active {
        color: @sc-primary-color;
        font-weight: bold;
      }
    }
    &:hover {
      overflow-y: auto;
    }
  }
}
</style>
