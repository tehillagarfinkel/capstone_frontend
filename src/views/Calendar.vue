<template>
  <div>
    <FullCalendar
      defaultView="dayGridMonth"
      :plugins="calendarPlugins"
      :events="calendarTasks"
      :editable="true"
      v-on:eventDrop="dropEvent"
    />
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
      tasks: [],
      events: []
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
    axios.get("/api/calendars").then(response => {
      console.log(response.data);
      this.events = response.data.calendar_events;
    });
  },
  computed: {
    calendarTasks: function() {
      const allTasks = this.tasks.map(task => {
        return { title: task.description, date: task.due_date };
      });
      this.events.forEach(event => {
        allTasks.push({ title: event.summary, date: event.start });
      });
      return allTasks;
    }
  },
  methods: {
    dropEvent: function(data) {
      console.log("dropEvent", data);
    }
  }
};
</script>
