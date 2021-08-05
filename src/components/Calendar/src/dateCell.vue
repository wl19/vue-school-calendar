<template>
  <div class="schedule-calendar-date" :class="[type, { today: isToday }]">
    <div class="schedule-calendar-date-hd">
      <div class="schedule-calendar-date-label">{{ date.getDate() }}</div>
    </div>
  </div>
</template>
<script>
import { defineComponent, computed } from "vue";
import { isSameDay } from "./utils";

export default defineComponent({
  props: {
    date: Date,
    type: String,
    data: Array,
  },
  setup(props) {
    const isToday = computed(() => {
      return isSameDay(new Date(), props.date);
    });
    return {
      isToday,
    };
  },
});
</script>
<style lang="less" scoped>
@import "./variables.less";

.schedule-calendar- {
  &date {
    position: relative;
    display: flex;
    flex-direction: column;
    width: ~"calc(100% / 7)";
    height: ~"calc(100% / 6)";
    min-width: @sc-cell-min-width;
    min-height: @sc-cell-min-height;
    padding: 4px;
    // border-top: 1px solid @sc-border-color;
    // border-left: 1px solid @sc-border-color;
    user-select: none;

    &:nth-child(7n + 1) {
      border-left: none;
    }

    &.prev,
    &.next {
      color: @sc-gray-light-color;
      // background: @sc-gray-background;
    }

    &-label {
      width: @sc-data-label-size;
      height: @sc-data-label-size;
      line-height: @sc-data-label-size;
      text-align: center;
      border-radius: 50%;
    }

    &.today {
      .schedule-calendar-date-label {
        color: @sc-body-color;
        background: @sc-primary-color;
      }
    }
  }
  &date-hd {
    display: flex;
    justify-content: center;
    align-content: center;
  }
}
</style>
