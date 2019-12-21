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

    <v-snackbar v-model="snackbar" :color="snackbarColor" bottom right>
      <div class="d-flex flex-row justify-start align-center" style="width:100%;height:100%;">
        <v-icon dark v-text="snackbarIcon" />
        <div class="mx-5">{{snackbarMessage}}</div>
      </div>
    </v-snackbar>
  </v-container>
</template>

<script>
import Schedule from "./Schedule";
export default {
  data() {
    return {
      snackbarColor: "",
      snackbarIcon: "",
      snackbarMessage: "",
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
      cloneSchedule: [],
      cloneWorkingTime: []
    };
  },
  props: {
    dark: Object,
    user: Object,
    schedule: Object,
    staffs: Object
  },
  components: {
    Schedule
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    onEditSchedule() {
      this.isEditingSchedule = !this.isEditingSchedule;
      this.cloneSchedule = JSON.parse(JSON.stringify(this.schedule));
    },
    onSaveSchedule(schedule) {
      this.isEditingSchedule = !this.isEditingSchedule;
      this.schedule = schedule;
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
      this.showSnackbar("success", "mdi-check", "Changes Saved!");
    },
    onSelectTime(weekday, time) {
      this.user.workingTime[weekday][time] = !this.user.workingTime[weekday][
        time
      ];
      this.$forceUpdate();
    }
  },
  created() {

  }
};
</script>

<style>
</style>