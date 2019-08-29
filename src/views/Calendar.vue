<template>
  <div>
    <FullCalendar
      defaultView="timeGridWeek"
      :plugins="calendarPlugins"
      :events="calendarTasks"
      :editable="true"
      :timeZone="`local`"
      v-on:eventDrop="dropEvent"
      v-on:eventClick="eventClick"
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
      events: [{}],
      colors: ["#f0c24b", "#b5d56a", "#ea7066", "#84bed6", "#a597e7", "#ea77ad"],
      categoryColors: {},
      colorIndex: 0
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
          this.categoryColors[category.id] = this.colors[this.colorIndex];
          this.colorIndex += 1;
          if (this.colorIndex >= this.colors.length) {
            this.colorIndex = 0;
          }
          category.tasks.forEach(task => {
            this.tasks.push(task);
          });
        });
        console.log("categoryColors", this.categoryColors);
      });
    });
  },
  computed: {
    calendarTasks: function() {
      let allTasks = this.tasks.map(task => {
        var backgroundColor = "#808080";
        if (task.start === null) {
          backgroundColor = "#808080";
        } else {
          backgroundColor = this.categoryColors[task.category_id];
        }

        return {
          title: `${task.description} - due ${task.due_date}`,
          start: task.start || this.events[0].end,
          id: task.id,
          backgroundColor: backgroundColor,
          category_id: 1
        };
      });
      this.events.forEach(event => {
        allTasks.push({
          title: event.summary,
          start: event.start,
          end: event.end,
          backgroundColor: "#004d4d"
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
        // console.log(response.data);
        var task = response.data;
        data.event.setProp("backgroundColor", this.categoryColors[task.category_id]);
      });
    },
    eventClick: function(data) {
      console.log("Event: " + data.event.title);
      // alert("Coordinates: " + data.jsEvent.pageX + "," + data.jsEvent.pageY);
      // alert("View: " + data.view.type);
    },
    getRandomColor: function() {
      var letters = "0123456789ABCDEF";
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    }
  }
};
</script>
