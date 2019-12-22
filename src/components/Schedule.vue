<template>
  <v-card :dark="dark[0]">
    <v-card-text>
      <v-row>
        <v-col class="ma-0 py-0" cols="12" v-if="isEditing">
          <div class="my-1 caption">1. Click a staff to see which working time is prefered.</div>
          <div class="my-1 caption">2. Drag and drop the staff to the schedule below.</div>
          <div class="my-1 caption">3. You can also swap between the blocks.</div>
          <v-chip-group
            class="mb-1"
            active-class="primary--text"
            v-model="selectedStaffIndex"
            column
            mandatory
          >
            <drag
              v-for="(staff, i) in staffs"
              :key="i"
              :transfer-data="{isDeleted: false, staff: staff}"
              :effect-allowed="['copy']"
              drop-effect="copy"
            >
              <v-chip style="cursor:move" outlined :style="staff.role==0?'display:none':''">
                <v-avatar :color="staff.color" left>
                  <span class="white--text" v-text="`${staff.firstName[0]}${staff.lastName[0]}`" />
                </v-avatar>
                {{`${staff.firstName} ${staff.lastName}`}}
              </v-chip>
            </drag>
          </v-chip-group>
        </v-col>

        <v-col class="ma-0 pb-5" cols="12" v-if="isEditing">
          <v-divider />
        </v-col>

        <v-col class="ma-0 py-0 d-flex flex-column justify-space-around" cols="2">
          <v-sheet class="mb-5" height="50" color="transparent" />
          <v-sheet
            class="mb-5 d-flex justify-center align-center"
            height="100"
            :color="dark[0]?'grey darken-4':'grey lighten-5'"
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
              class="mx-2 pa-0 d-flex flex-column justify-space-around align-center"
              v-for="(workday, i) in schedule"
              :key="i"
            >
              <v-sheet
                class="mb-5 d-flex flex-column justify-space-around align-center"
                width="100%"
                height="100"
                v-for="(time, j) in workday.length"
                :key="j"
                :color="setBackgroundColor(i, j)"
              >
                <drag
                  style="width:100%;height:100%;"
                  :style="isEditing?'cursor:move':'cursor:default'"
                  :draggable="isEditing"
                  :transfer-data="{isParent:true, day:i, time:j}"
                  :effect-allowed="['link']"
                  drop-effect="link"
                >
                  <drop
                    class="ma-0 pa-0 d-flex flex-column justify-space-around align-center"
                    style="width:100%;height:100%;"
                    @drop="handleDrop(i,j, ...arguments)"
                  >
                    <drag
                      :draggable="isEditing"
                      v-for="(staff, k) in workday[j]"
                      :key="k"
                      :transfer-data="{isDeleted:true, day:i, time:j, index: k, staff: staff}"
                      :effect-allowed="['link']"
                      drop-effect="link"
                      @dragstart="handleChildDragstart"
                    >
                      <v-chip
                        class="my-1 hidden-sm-and-down"
                        :close="isEditing"
                        :style="isEditing?'cursor:move':'cursor:default'"
                        outlined
                        @click:close="onClickClose(i, j, k)"
                      >
                        <v-avatar left :color="staff.color">
                          <span
                            class="white--text"
                            v-text="`${staff.firstName[0]}${staff.lastName[0]}`"
                          />
                        </v-avatar>
                        {{staff.id}}
                      </v-chip>

                      <v-avatar
                        class="d-md-none"
                        size="32"
                        v-for="(staff, k) in workday[j]"
                        :key="k"
                        :color="staff.color"
                      >
                        <span
                          class="white--text"
                          v-text="`${staff.firstName[0]}${staff.lastName[0]}`"
                        />
                      </v-avatar>
                    </drag>
                  </drop>
                </drag>
              </v-sheet>
            </v-col>
          </v-col>
        </v-col>

        <v-col class="ma-0 py-0" cols="12" v-if="isEditing">
          <v-divider />
        </v-col>

        <v-col class="d-flex flex-row justify-end" cols="12" v-if="isEditing">
          <v-btn class="mx-2" text @click="onClickCancel()">Cancel</v-btn>
          <v-btn class="mx-2" color="primary" text @click="onClickSaveChanges()">Save</v-btn>
        </v-col>
      </v-row>

      <v-snackbar v-model="snackbar" :color="snackbarColor" bottom right>
        <div class="d-flex flex-row justify-start align-center" style="width:100%;height:100%;">
          <v-icon dark v-text="snackbarIcon" />
          <div class="mx-5">{{snackbarMessage}}</div>
        </div>
      </v-snackbar>
    </v-card-text>
  </v-card>
</template>

<style>
</style>

<script>
import { Drag, Drop } from "vue-drag-drop";

export default {
  name: "Schedule",
  data() {
    return {
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
      selectedStaffIndex: 0,
      backgroundColor: "grey lighten-5",
      snackbar: false,
      snackbarColor: "",
      snackbarIcon: "",
      snackbarMessage: ""
    };
  },
  props: {
    dark: Object,
    isEditing: Boolean,
    schedule: Object,
    staffs: Object
  },
  components: {
    Drag,
    Drop
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    onClickCancel() {
      this.$emit("cancelFromSchedule");
    },
    onClickSaveChanges() {
      this.showSnackbar("success", "mdi-check", "Changes Saved!");
      this.$emit("saveFromSchedule", this.schedule);
    },
    onClickClose(day, time, index) {
      this.schedule[day][time].splice(index, 1);
    },
    setBackgroundColor(i, j) {
      if (!this.isEditing) {
        return this.dark[0] ? "grey darken-4" : "grey lighten-5";
      } else {
        if (this.staffs[this.selectedStaffIndex].workingTime[i][j]) {
          return this.dark[0] ? "blue lighten-2" : "blue lighten-5";
        } else {
          return this.dark[0] ? "red lighten-2" : "red lighten-5";
        }
      }
    },
    handleChildDragstart(data, event) {
      event.stopPropagation();
    },
    handleDrop(day, time, data) {
      if (this.isEditing && data) {
        if (day != data.day || time != data.time) {
          if (data.isParent) {
            let isStaffExist = false;
            if (day != data.day) {
              for (let t in this.schedule[day]) {
                if (t != time) {
                  for (let staff of this.schedule[day][t]) {
                    for (let staff1 of this.schedule[data.day][data.time]) {
                      if (staff.id == staff1.id) {
                        isStaffExist = true;
                        break;
                      }
                    }
                  }
                }
              }

              for (let t in this.schedule[data.day]) {
                if (t != data.time) {
                  for (let staff of this.schedule[data.day][t]) {
                    for (let staff1 of this.schedule[day][time]) {
                      if (staff.id == staff1.id) {
                        isStaffExist = true;
                        break;
                      }
                    }
                  }
                }
              }
            }

            if (!isStaffExist) {
              let temp = this.schedule[day][time];
              this.schedule[day][time] = this.schedule[data.day][data.time];
              this.schedule[data.day][data.time] = temp;
            } else {
              this.showSnackbar(
                "error",
                "mdi-alert",
                "One of the staffs is already scheduled in the day!"
              );
            }
          } else if (this.schedule[day][time].length < 2) {
            let isStaffExist = false;
            if (day != data.day) {
              for (let t of this.schedule[day]) {
                for (let staff of t) {
                  if (staff.id == data.staff.id) {
                    isStaffExist = true;
                    break;
                  }
                }
              }
            }

            if (!isStaffExist) {
              if (data.isDeleted) {
                this.schedule[data.day][data.time].splice(data.index, 1);
              }
              if (data.staff) {
                this.schedule[day][time].push(data.staff);
              }
            } else {
              this.showSnackbar(
                "error",
                "mdi-alert",
                "The staff you selected already scheduled in the day!"
              );
            }
          } else {
            this.showSnackbar(
              "error",
              "mdi-alert",
              "Only two staffs are allowed to scheduled in the same time!"
            );
          }
          this.$forceUpdate();
        }
      }
    }
  }
};
</script>