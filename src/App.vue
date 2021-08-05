<template>
  <div style="height: 400px">
    <Calendar :events="events"></Calendar>
  </div>
</template>

<script >
import { defineComponent ,reactive} from 'vue'
import Calendar from './components/Calendar'
export default defineComponent({
  components: { Calendar },
  setup(){
    const events = reactive([
      {
        id: 1,
        date: '2021-07-31',
        text: '开学',
        background: '#ff00ff'
      },
      {
        id: 2,
        date: '2021-08-11',
        text: '放假',
        background: '#ffff00'
      },
    ])
    return {
      events
    }
  }
})
</script>

