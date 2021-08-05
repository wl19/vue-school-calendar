<template>
  <div style="height: 400px">
    <Calendar :events="events" :terms="terms" title="2021-2022学年 第一学期"></Calendar>
  </div>
</template>

<script>
import { defineComponent, reactive } from "vue";
import Calendar from "./components/Calendar";
export default defineComponent({
  components: { Calendar },
  setup() {
    const events = reactive([
      {
        term: 72,
        title: "开学",
        start: "2021-07-29",
      },
      {
        term: 72,
        title: "放假",
        start: "2021-08-29",
      },
      {
        term: 72,
        title: "今天",
        start: "2021-08-05",
      },
    ]);

    const terms = reactive([
      {
        term: 72,
        title: "2021-2022学年 第一学期",
        start: "2021-07-26",
        end: "2021-08-29",
        beforeStartDate: "2021-07-29",
        beforeEndDate: "2021-08-29",
      },
    ]);

    return {
      events,
      terms
    };
  },
});
</script>
