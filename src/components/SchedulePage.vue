<template>
  <v-container>
    <v-row align-content="start" class="px-5">
      <v-col cols="12">
        <v-row justify="start">
          <div class="display-1">Schedule</div>
        </v-row>
      </v-col>
      <v-col class="mx-0 px-0" cols="12">
        <v-divider :dark="dark[0]" />
      </v-col>

      <v-col cols="12" lg="10" xl="6" v-if="user.role == 0">
        <div class="mb-2 text-left title">Calendar</div>
        <div class="mb-2 d-flex justify-space-between align-center">
          <div class="text-left body-2">Manage the events</div>
          <v-btn color="primary" @click="onEditEvents()">Edit</v-btn>
        </div>

        <v-col class="ma-0 pa-0" cols="12">
          <Calendar :dark="dark" :events="events" />
        </v-col>
      </v-col>

      <v-col cols="12" xl="10">
        <div v-if="user.role == 0">
          <div class="mb-2 text-left title">Staffs' Working Time</div>
          <div class="mb-2 d-flex justify-space-between align-center">
            <div class="text-left body-2">Manage the schedule</div>
            <v-btn v-if="!isEditingSchedule" color="primary" @click="onEditSchedule()">Edit</v-btn>
          </div>
          <Schedule
            :dark="dark"
            :isEditing="isEditingSchedule"
            :schedule="schedule"
            :staffs="staffs"
            @saveFromSchedule="onSaveSchedule"
            @cancelFromSchedule="onCancelSchedule"
          />
        </div>
        <div v-if="user.role == 1">
          <!-- <div class="mb-2 text-left title">Working Time</div> -->
          <div class="mb-2 d-flex justify-space-between align-center">
            <div class="text-left body-2">Select the working time you prefer</div>
            <v-btn v-if="!isEditingWorkingTime" color="primary" @click="onEditWorkingTime()">Edit</v-btn>
          </div>
          <v-card class="mb-5" :dark="dark[0]">
            <v-card-text>
              <v-row>
                <v-col class="ma-0 py-0 d-flex flex-column justify-space-around" cols="2">
                  <v-sheet class="mb-5" height="50" color="transparent" />
                  <v-sheet
                    class="mb-5 d-flex justify-center align-center"
                    height="50"
                    :color="dark[0]?'grey darken-4':'grey lighten-4'"
                    v-for="(item, i) in times"
                    :key="i"
                  >
                    <div class="body-1" v-text="item.text" />
                  </v-sheet>
                </v-col>

                <v-col class="ma-0 pa-0" cols="10">
                  <v-col class="ma-0 py-0 d-flex flex-row justify-space-around" cols="12">
                    <v-sheet
                      class="mx-2 mb-5 d-flex justify-center align-center"
                      width="100%"
                      height="50"
                      v-for="(item, i) in weekdays"
                      :key="i"
                      :color="item.color"
                    >
                      <div class="body-1 white--text" v-text="item.text" />
                    </v-sheet>
                  </v-col>

                  <v-col class="ma-0 py-0 d-flex flex-row justify-space-around" cols="12">
                    <v-col
                      class="mx-2 pa-0 d-flex flex-column"
                      v-for="(workday, i) in user.workingTime"
                      :key="i"
                    >
                      <v-btn
                        class="mb-5 d-flex justify-center align-center"
                        width="100%"
                        height="50"
                        :color="weekdays[i].color"
                        :outlined="isEditingWorkingTime"
                        :disabled="!isEditingWorkingTime"
                        v-for="(time, j) in workday.length"
                        :key="j"
                        @click="onSelectTime(i, j)"
                      >
                        <v-icon color="green" v-if="user.workingTime[i][j]">mdi-check</v-icon>
                        <v-icon color="red" v-else>mdi-close</v-icon>
                      </v-btn>
                    </v-col>
                  </v-col>
                </v-col>
              </v-row>

              <div v-if="isEditingWorkingTime">
                <v-divider />
                <div class="d-flex justify-end">
                  <v-btn class="mt-5 me-5" text @click="onCancelWorkingTime()">Cancel</v-btn>
                  <v-btn class="mt-5" color="primary" text @click="onSaveWorkingTime()">Save</v-btn>
                </div>
              </div>
            </v-card-text>
          </v-card>
        </div>
      </v-col>
    </v-row>

    <v-dialog width="500" v-model="eventDialog" v-if="user.role == 0" scrollable>
      <v-card :dark="dark[0]">
        <v-card-title>
          <span class="headline primary--text">Manage The Events</span>
          <v-spacer />
          <v-btn text icon large color="green" @click="onAddEvent()">
            <v-icon large>mdi-plus</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col class="ma-0 py-0" cols="12" v-for="(event, i) in events" :key="i">
              <v-col class="pa-0 d-flex flex-row justify-start align-center" cols="12">
                <v-sheet class="mx-3" width="20px" height="20px" :color="event.color" />
                <span class="ms-1 body-2" v-text="event.name"></span>
                <v-spacer />
                <span class="me-2 body-2" v-text="event.start"></span>
                <span class="me-2 body-2">~</span>
                <span class="me-2 body-2" v-text="event.end"></span>
                <v-btn class="me-1" text icon color="red" @click="onRemoveEvent(i)">
                  <v-icon>mdi-close</v-icon>
                </v-btn>
              </v-col>
              <v-divider />
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn text @click="onCancelEvents()">Cancel</v-btn>
          <v-btn color="primary" text @click="onSaveEvents()">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog width="500" v-model="dateDialog" v-if="user.role == 0" scrollable>
      <v-card :dark="dark[0]">
        <v-card-title>
          <span class="headline primary--text">Add New Event</span>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col class="py-0" cols="12">
              <v-text-field v-model="eventName" label="Event Name"></v-text-field>
            </v-col>
            <v-col class="py-0" cols="12">
              <v-menu transition="scale-transition" offset-y min-width="290px">
                <template v-slot:activator="{ on }">
                  <v-text-field v-model="startDate" label="Start Date" readonly v-on="on" />
                </template>
                <v-date-picker v-model="startDate" :dark="dark[0]" no-title scrollable />
              </v-menu>
            </v-col>
            <v-col class="py-0" cols="12">
              <v-menu transition="scale-transition" offset-y min-width="290px">
                <template v-slot:activator="{ on }">
                  <v-text-field v-model="endDate" label="End Date" readonly v-on="on" />
                </template>
                <v-date-picker v-model="endDate" :dark="dark[0]" no-title scrollable />
              </v-menu>
            </v-col>
            <v-col class="py-0 d-flex flex-row justify-start" cols="12">
              <v-sheet
                class="me-5 d-flex justify-center align-center"
                width="20"
                height="20"
                style="cursor:pointer"
                v-for="(sheet, i) in colors"
                :color="sheet.color"
                :key="i"
                @click="onSelectEventColor(i)"
              >
                <v-icon size="15" v-if="sheet.isSelected" dark>mdi-check</v-icon>
              </v-sheet>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn text @click="dateDialog = false">Cancel</v-btn>
          <v-btn color="primary" text @click="onSaveAddEvent()">Done</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-snackbar v-model="snackbar" :color="snackbarColor" bottom right>
      <div class="d-flex flex-row justify-start align-center" style="width:100%;height:100%;">
        <v-icon dark v-text="snackbarIcon" />
        <div class="mx-5">{{snackbarMessage}}</div>
      </div>
    </v-snackbar>
  </v-container>
</template>

<script>
import Calendar from "./Calendar";
import Schedule from "./Schedule";
export default {
  data() {
    return {
      snackbarColor: "",
      snackbarIcon: "",
      snackbarMessage: "",
      eventDialog: false,
      dateDialog: false,
      eventName: "",
      startDate: "",
      endDate: "",
      selectedColorIndex: 0,
      colors: [
        {
          isSelected: true,
          color: "pink lighten-2"
        },
        {
          isSelected: false,
          color: "deep-purple lighten-2"
        },
        {
          isSelected: false,
          color: "blue lighten-2"
        },
        {
          isSelected: false,
          color: "cyan lighten-2"
        },
        {
          isSelected: false,
          color: "green lighten-2"
        },
        {
          isSelected: false,
          color: "lime lighten-2"
        },
        {
          isSelected: false,
          color: "amber lighten-2"
        },
        {
          isSelected: false,
          color: "deep-orange lighten-2"
        },
        {
          isSelected: false,
          color: "blue-grey lighten-2"
        }
      ],
      isEditingSchedule: false,
      isEditingWorkingTime: false,
      weekdays: [
        {
          text: "Sun",
          color: "pink lighten-2"
        },
        {
          text: "Mon",
          color: "deep-purple lighten-2"
        },
        {
          text: "Tue",
          color: "blue lighten-2"
        },
        {
          text: "Wed",
          color: "cyan lighten-2"
        },
        {
          text: "Thu",
          color: "green lighten-2"
        },
        {
          text: "Fri",
          color: "lime lighten-2"
        },
        {
          text: "Sat",
          color: "amber lighten-2"
        }
      ],
      times: [
        {
          text: "00 - 08"
        },
        {
          text: "08 - 16"
        },
        {
          text: "16 - 24"
        }
      ],
      cloneEvents: [],
      cloneSchedule: [],
      cloneWorkingTime: []
    };
  },
  props: {
    dark: Object,
    user: Object,
    schedule: Object,
    staffs: Object,
    events: Object,
    history: Object
  },
  components: {
    Calendar,
    Schedule
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    addNewHistory(content) {
      let date = new Date();
      this.history.push({
        id: this.user.id,
        firstName: this.user.firstName,
        lastName: this.user.lastName,
        color: this.user.color,
        time: `${date.getHours()}:${date.getMinutes()}`,
        content: content
      });
    },
    onEditSchedule() {
      this.isEditingSchedule = !this.isEditingSchedule;
      this.cloneSchedule = JSON.parse(JSON.stringify(this.schedule));
    },
    onSaveSchedule(schedule) {
      this.isEditingSchedule = !this.isEditingSchedule;
      this.schedule = schedule;
      this.addNewHistory("Release new schedule");
    },
    onCancelSchedule() {
      this.isEditingSchedule = !this.isEditingSchedule;
      this.schedule = this.cloneSchedule;
    },
    onEditWorkingTime() {
      this.cloneWorkingTime = JSON.parse(JSON.stringify(this.user.workingTime));
      this.isEditingWorkingTime = !this.isEditingWorkingTime;
    },
    onCancelWorkingTime() {
      this.user.workingTime = this.cloneWorkingTime;
      this.isEditingWorkingTime = !this.isEditingWorkingTime;
    },
    onSaveWorkingTime() {
      this.isEditingWorkingTime = !this.isEditingWorkingTime;
      this.addNewHistory(`Update Working Time`);
      this.showSnackbar("success", "mdi-check", "Changes Saved!");
    },
    onSelectTime(weekday, time) {
      this.user.workingTime[weekday][time] = !this.user.workingTime[weekday][
        time
      ];
      this.$forceUpdate();
    },
    onEditEvents() {
      this.eventDialog = true;
      this.cloneEvents = JSON.parse(JSON.stringify(this.events));
    },
    onCancelEvents() {
      this.eventDialog = false;
      this.events = this.cloneEvents;
    },
    onSaveEvents() {
      this.eventDialog = false;
      this.showSnackbar("success", "mdi-check", "Changes Saved!");
    },
    onAddEvent() {
      this.dateDialog = true;
      this.onSelectEventColor(0);
      this.eventName = "";
      this.startDate = "";
      this.endDate = "";
    },
    onSelectEventColor(index) {
      this.selectedColorIndex = index;
      for (let i in this.colors) {
        if (i == index) {
          this.colors[i].isSelected = true;
        } else {
          this.colors[i].isSelected = false;
        }
      }
    },
    onSaveAddEvent() {
      this.dateDialog = false;
      this.events.push({
        name: this.eventName,
        start: this.startDate,
        end: this.endDate,
        color: this.colors[this.selectedColorIndex].color
      });
      this.events.sort((a, b) => {
        return a.start > b.start ? 1 : -1;
      });

      this.addNewHistory(`Add new event - ${this.eventName}`);
    },
    onRemoveEvent(i) {
      this.addNewHistory(`Remove event - ${this.events[i].name}`);
      this.events.splice(i, 1);
    }
  },
  created() {}
};
</script>

<style>
</style>