<template>
  <div
    class="schedule-calendar-month"
    :class="{ [animationClass]: animated }"
    @animationend="removeAnimation"
  >
    <date-cell
      v-for="(item, index) in days"
      :date="item.date"
      :type="item.type"
      :data="data"
      :key="index"
    ></date-cell>
  </div>
</template>
<script>
import { defineComponent, reactive, toRefs, computed, watch } from "vue";
import { monthlyCalendar } from "./utils";
import dateCell from "./dateCell.vue";

export default defineComponent({
  components: {
    dateCell,
  },
  props: {
    year: Number,
    month: Number,
    startWeek: Number,
    data: Array,
  },
  setup(props) {
    const state = reactive({
      viewTransition: "sc-moveTo",
      direction: "Left",
      animated: false,
    });
    const days = computed(() => {
      return monthlyCalendar(props.year, props.month, props.startWeek);
    });
    const animationClass = computed(() => {
      return state.viewTransition + state.direction;
    });
    const mDate = computed(() => {
      return new Date(props.year, props.month, 0);
    });

    const removeAnimation = () => {
      state.animated = false;
    };
    const addAnimation = (val, old) => {
      if (val !== old) {
        state.animated = true;
      }
    };
    

    watch(
      () => mDate.value,
      (val, old) => {
        if (old) {
          if (val < old) {
            state.direction = "Right";
          }
          if (val > old) {
            state.direction = "Left";
          }
        }
        if (val !== old) {
          state.animated = true;
        }
      }
    );

    return {
      ...toRefs(state),
      days,
      animationClass,
      mDate,
      removeAnimation,
      addAnimation,
    };
  },
});
</script>
<style lang="less" scoped>
@import "./variables.less";

.schedule-calendar- {
  &month {
    position: absolute;
    top: @sc-week-height;
    left: 0;
    right: 0;
    bottom: 40px;
    display: flex;
    flex-wrap: wrap; // transition: transform .3s ease-in-out;
  }
}

.sc-moveToLeft {
  animation: scMoveToLeft 0.3s both;
}

.sc-moveToRight {
  animation: scMoveToRight 0.3s both;
}

@keyframes scMoveToLeft {
  from {
    transform: translate3d(50%, 0, 0);
    visibility: visible;
  }

  to {
    transform: translate3d(0, 0, 0);
  }
}

@keyframes scMoveToRight {
  from {
    transform: translate3d(-50%, 0, 0);
    visibility: visible;
  }

  to {
    transform: translate3d(0, 0, 0);
  }
}
</style>
