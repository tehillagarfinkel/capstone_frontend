<template>
  <FullCalendar
    defaultView="dayGridMonth"
    :plugins="calendarPlugins"
    :events="[
      { title: 'event 1', date: '2019-08-01' },
      { title: 'event 2', date: '2019-08-02' },
      { title: 'event 3', date: this.tasks }
    ]"
  />
</template>

<style lang="scss">
@import "~@fullcalendar/core/main.css";
@import "~@fullcalendar/daygrid/main.css";
</style>

<script>
import axios from "axios";
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarPlugins: [dayGridPlugin],
      tasks: []
    };
  },
  created: function() {
    axios.get("/api/categories/1").then(response => {
      // console.log(response.data);
      // // this.category = response.data;
      // // this.tasks = response.data.tasks;
      // console.log(this.tasks[0].due_date);
      this.tasks = response.data.tasks[0].due_date;
      console.log("task is:", this.tasks);
    });
  }
};
</script>
