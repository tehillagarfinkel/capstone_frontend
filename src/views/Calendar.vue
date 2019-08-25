<template>
  <div>
    <FullCalendar defaultView="dayGridMonth" :plugins="calendarPlugins" :events="calendarTasks" />
  </div>
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
    axios.get("/api/categories").then(response => {
      console.log(response.data);
      var categories = response.data;
      categories.forEach(category => {
        category.tasks.forEach(task => {
          this.tasks.push(task);
        });
      });
    });
  },
  computed: {
    calendarTasks: function() {
      return this.tasks.map(task => {
        return { title: task.description, date: task.due_date };
      });
    }
  }
};
</script>
