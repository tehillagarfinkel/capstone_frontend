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
            <h5 class="modal-title" id="exampleModalLabel">{{ currentTask.description }}</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div>Due: {{ currentTask.formatted.due_date }}</div>
            <div>Start Time: {{ currentTask.formatted.start }}</div>
            <div>Duration: {{ currentTask.duration }}</div>
            <div>Completed: {{ currentTask.completed }}</div>
            <!-- <div>
              I'm done:
              <input v-on:click="markComplete(currentTask)" type="checkbox" />
            </div> -->
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-primary bg-color-1"
              data-dismiss="modal"
              v-on:click="markComplete(currentTask)"
            >
              {{ currentTask.description }} is done!
            </button>
            <button type="button" class="btn btn-primary bg-color-3" data-dismiss="modal">Close</button>
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

<style>
.completed {
  opacity: 0.5;
}
</style>

<script>
/* global $ */

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
      currentTask: { formatted: {} },
      currentDataEvent: null,
      nextValidStartTime: new Date()
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
        console.log("tasks", this.tasks);
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

        var startEnd = this.getValidStartEnd(task);
        return {
          title: task.description,
          start: startEnd[0],
          end: startEnd[1],
          id: task.id,
          backgroundColor: backgroundColor,
          category_id: 1,
          completed: task.completed,
          dueDate: task.due_date,
          className: task.completed ? "completed" : ""
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
      this.currentTask = this.tasks.find(task => task.id === parseInt(data.event.id));
      if (this.currentTask) {
        console.log(this.currentTask);
        this.currentDataEvent = data.event;
        $("#exampleModal").modal();
      } else {
        this.currentTask = {};
      }
      console.log("current task is", this.currentTask);
    },
    markComplete: function(currentTask) {
      if (this.currentTask.completed === true) {
        this.currentTask.completed = false;
      } else {
        this.currentTask.completed = true;
      }
      if (this.currentDataEvent) {
        this.currentDataEvent.setProp("className", this.currentTask.completed ? "completed" : "");
      }
      console.log("currentTask:", currentTask);
      console.log("taskId", currentTask.id);
      console.log("completed", currentTask.completed);

      var params = {
        completed: this.currentTask.completed
      };
      // Object.keys(params).forEach(key => params[key] === "" && delete params[key]);
      console.log("START PATCH /api/tasks", params);
      axios
        .patch("/api/tasks/" + currentTask.id, params)
        .then(response => {
          console.log("FINISH PATCH /api/tasks", response.data);
          this.currentTask = response.data;
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    getValidStartEnd: function(task) {
      // console.log(task.start);
      // var start = task.start || this.events[0].end;
      // var end = new Date(new Date(start).getTime() + task.duration * 60000);
      // return [start, end];
      this.events.forEach(event => {
        var eventStart = new Date(event.start);
        var eventEnd = new Date(event.end);
        console.log(eventStart, eventEnd, this.nextValidStartTime);
        if (eventStart <= this.nextValidStartTime && eventEnd >= this.nextValidStartTime) {
          this.nextValidStartTime = event.end;
        }
      });
      var start = task.start || this.nextValidStartTime;
      var end = new Date(new Date(start).getTime() + task.duration * 60000);
      this.nextValidStartTime = end;
      return [start, end];
    }
  }
};
</script>
