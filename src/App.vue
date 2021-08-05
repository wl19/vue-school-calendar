<template>
  <div style="height: 400px">
    <Calendar></Calendar>
  </div>
</template>

<script >
import { defineComponent } from 'vue'
import Calendar from './components/Calendar'
export default defineComponent({
  components: { Calendar }
})
</script>

