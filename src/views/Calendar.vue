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
          start: this.events[0].end
        };
      });
      this.events.forEach(event => {
        allTasks.push({
          title: event.summary,
          start: event.start,
          end: event.end,
          backgroundColor: "#f00"
        });
        console.log("event.start", event.start, "formatDate", moment(event.start).toDate());
      });
      // allTasks = [
      //   { title: "Meeting", start: "2019-08-28T10:30:00+00:00", end: "2019-08-28T12:30:00+00:00" },
      //   { title: "Lunch", start: "2019-08-28T12:00:00+00:00" },
      //   { title: "Birthday Party", start: "2019-08-29T07:00:00+00:00" },
      //   { url: "http://google.com", title: "Click for Google", start: "2019-08-28" }
      // ];
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
