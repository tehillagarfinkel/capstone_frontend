<template>
  <div>
    <FullCalendar
      defaultView="dayGridMonth"
      :plugins="calendarPlugins"
      :events="calendarTasks"
      :editable="true"
      v-on:eventDrop="dropEvent"
    />
    <!-- <FullCalendar defaultView="dayGridMonth" :plugins="[calendarPlugins, interactionPlugin ]" :events="calendarTasks"
    editable: true /> -->
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
import interactionPlugin from "@fullcalendar/interaction";

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarPlugins: [dayGridPlugin, interactionPlugin],
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
  },
  methods: {
    dropEvent: function(data) {
      console.log("dropEvent", data);
    }
  }
};
</script>
