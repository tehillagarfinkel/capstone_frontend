<template>
  <div>
    <FullCalendar
      defaultView="timeGridWeek"
      :plugins="calendarPlugins"
      :events="calendarTasks"
      :editable="true"
      :timeZone="`local`"
      v-on:eventDrop="dropEvent"
    />
  </div>
</template>

<style lang="scss">
@import "~@fullcalendar/core/main.css";
@import "~@fullcalendar/daygrid/main.css";
@import "~@fullcalendar/timegrid/main.css";
</style>

<script>
import axios from "axios";
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";
import { formatDate } from "@fullcalendar/core";
import moment from "moment";

export default {
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarPlugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
      tasks: [],
      events: [{}]
    };
  },
  created: function() {
    axios.get("/api/calendars").then(response => {
      console.log("events", response.data);
      this.events = response.data.calendar_events;

      axios.get("/api/categories").then(response => {
        console.log(response.data);
        var categories = response.data;
        categories.forEach(category => {
          category.tasks.forEach(task => {
            this.tasks.push(task);
          });
        });
      });
    });
  },
  computed: {
    calendarTasks: function() {
      let allTasks = this.tasks.map(task => {
        return {
          title: `${task.description} - due ${task.due_date}`,
          start: task.start || this.events[0].end,
          id: task.id
        };
      });
      this.events.forEach(event => {
        allTasks.push({
          title: event.summary,
          start: event.start,
          end: event.end,
          backgroundColor: "#f00"
        });
      });
      return allTasks;
    }
  },
  methods: {
    dropEvent: function(data) {
      console.log("dropEvent", data);
      console.log("the new event time is", data.event.start);
      console.log("the task id is:", data.event.id);
      var params = {
        start: data.event.start
      };
      axios.patch("/api/tasks/" + data.event.id, params).then(response => {
        console.log(response.data);
      });
    }
  }
};
</script>
