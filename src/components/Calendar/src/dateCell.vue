<template>
  <div class="schedule-calendar-date">
    <div class="schedule-calendar-date-hd">
      <div class="schedule-calendar-date-label" :style="eventStyle" :class="[type]">{{ date.getDate() }}</div>
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
    colorList: {
      type: Array,
      default: [],
    },
  },
  setup(props) {
    const isToday = computed(() => {
      return isSameDay(new Date(), props.date);
    });
    const eventStyle = computed(()=>{
      const style = {
        background: '#fff',
        color: props.type == 'current' ? '#444' : '#ccc'
      }
      if(props.data && props.data.length){
        for (let i = 0; i < props.data.length; i++) {
          let item = props.data[i]
          if(isSameDay(new Date(item.start),props.date)){
            return {
              background: item.background?item.background:props.colorList[i],
              color: item.color?item.color:'#fff'
            }
          }
        }
        return style
      }else{
        return style
      }
    })
    return {
      isToday,
      eventStyle
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

    

    &-label {
      width: @sc-data-label-size;
      height: @sc-data-label-size;
      line-height: @sc-data-label-size;
      text-align: center;
      border-radius: 50%;

      &.prev,
      &.next {
        // color: @sc-gray-light-color !important;
        // background: @sc-gray-background;
      }

      &.today {
        color: @sc-body-color !important;
        background: @sc-primary-color !important;
      }
    }
    
  }
  &date-hd {
    display: flex;
    justify-content: center;
    align-content: center;
    height: 100%;
    align-items: center;
  }
  
}
</style>
