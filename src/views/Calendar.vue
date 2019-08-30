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
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">{{ currentCalendarEvent.title }}</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div>Due: {{ currentCalendarEvent.dueDate }}</div>
            <div>Start Time: {{ currentCalendarEvent.start }}</div>
            <div>Completed: {{ currentCalendarEvent.completed }}</div>
            <div>
              I'm done:
              <input v-on:click="markComplete(currentCalendarEvent)" type="checkbox" />
            </div>
          </div>
          <div class="modal-footer">
            <!--  <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button> -->
            <button type="button" class="btn btn-primary" data-dismiss="modal">Save changes</button>
          </div>
        </div>
      </div>
    </div>
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
      colorIndex: 0,
      currentCalendarEvent: {}
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
          title: task.description,
          start: task.start || this.events[0].end,
          id: task.id,
          backgroundColor: backgroundColor,
          category_id: 1,
          completed: task.completed,
          dueDate: task.due_date
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
        var task = response.data;
        data.event.setProp("backgroundColor", this.categoryColors[task.category_id]);
      });
    },
    eventClick: function(data) {
      this.currentCalendarEvent = {
        title: data.event.title,
        start: data.event.start,
        completed: data.event.extendedProps.completed,
        dueDate: data.event.extendedProps.dueDate,
        taskId: data.event.id
      };
      $("#exampleModal").modal();
      console.log(data.event);
      console.log("due date is", data.event.extendedProps.dueDate);
    },
    markComplete: function(currentCalendarEvent) {
      if (this.currentCalendarEvent.completed === "true") {
        this.currentCalendarEvent.completed = "false";
      } else {
        this.currentCalendarEvent.completed = "true";
      }
      console.log("currentCalendarEvent:", currentCalendarEvent);
      console.log("taskId", currentCalendarEvent.taskId);
      console.log("completed", currentCalendarEvent.completed);

      var params = {
        completed: this.currentCalendarEvent.completed
      };
      Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      axios
        .patch("/api/tasks/" + currentCalendarEvent.taskId, params)
        .then(response => {
          console.log(response.data);
          this.currentCalendarEvent = response.data;
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
