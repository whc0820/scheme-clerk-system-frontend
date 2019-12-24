<template>
  <v-container>
    <v-row align-content="start" class="px-5">
      <v-col cols="12">
        <v-row justify="space-between">
          <div class="display-1">Staffs</div>
        </v-row>
      </v-col>

      <v-col class="mx-0 px-0" cols="12">
        <v-divider :dark="dark[0]" />
      </v-col>
      <v-col cols="12" lg="8">
        <div class="mb-4 text-left title">List of staffs</div>
        <div class="mb-4 d-flex justify-space-between align-center" v-if="user.role == 0">
          <div class="text-left body-2">Manage the staffs</div>
          <v-btn color="primary" @click="onEditStaffs()">Edit</v-btn>
        </div>
        <v-expansion-panels :dark="dark[0]">
          <v-expansion-panel v-for="(staff,i) in staffs" :key="i">
            <v-expansion-panel-header class="ma-0 px-4 py-4" v-if="staff.role == 1">
              <div class="ma-0 pa-0 d-flex flex-row justify-start align-center">
                <v-avatar :color="staff.color">
                  <span class="white--text" v-text="`${staff.firstName[0]}${staff.lastName[0]}`" />
                </v-avatar>
                <div class="ps-4 d-flex flex-column">
                  <div class="body-1" v-text="`${staff.firstName} ${staff.lastName}`" />
                  <div class="caption text-uppercase" v-text="staff.id" />
                </div>
              </div>
            </v-expansion-panel-header>
            <v-expansion-panel-content>
              <v-divider />
              <v-row class="mt-2">
                <v-col class="ms-1 d-flex flex-row justify-start align-center" cols="12">
                  <v-icon>mdi-phone</v-icon>
                  <div class="ms-7 caption" v-text="staff.phone" />
                </v-col>

                <v-col class="ms-1 d-flex flex-row justiy-start align-center" cols="12">
                  <v-icon>mdi-email</v-icon>
                  <div class="ms-7 caption" v-text="staff.email" />
                </v-col>
              </v-row>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-expansion-panels>
      </v-col>
    </v-row>

    <v-dialog width="500" v-model="editDialog" v-if="user.role == 0" scrollable>
      <v-card :dark="dark[0]">
        <v-card-title>
          <span class="headline primary--text">Manage the staffs</span>
          <v-spacer />
          <v-btn icon>
            <v-icon color="green" @click="staffDialog = true" large>mdi-plus</v-icon>
          </v-btn>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col class="py-0" cols="12" v-for="(staff,i) in staffs" :key="i">
              <div class="ms-3 pa-0 d-flex flex-row justify-start align-center" v-if="staff.role == 1">
                <v-sheet class="me-4" width="20" height="20" :color="staff.color" />
                <span class="me-3 body-2" v-text="staff.id" />
                <span class="body-2" v-text="`${staff.firstName} ${staff.lastName}`" />
                <v-spacer />
                <v-btn icon>
                  <v-icon color="red" @click="onRemoveStaff(i)">mdi-close</v-icon>
                </v-btn>
              </div>
              <v-divider v-if="staff.role == 1" />
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn text @click="onCancelStaffs()">Cancel</v-btn>
          <v-btn color="primary" text @click="onSaveStaffs()">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog width="500" v-model="staffDialog" v-if="user.role == 0">
      <v-card :dark="dark[0]">
        <v-card-title>
          <span class="headline primary--text">Add New Staff</span>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="6" class="py-0">
              <v-text-field v-model="firstName" label="First Name*" required />
            </v-col>
            <v-col cols="6" class="py-0">
              <v-text-field v-model="lastName" label="Last Name*" required />
            </v-col>
            <v-col cols="12" class="py-0">
              <v-text-field v-model="id" label="ID*" required />
            </v-col>
            <v-col cols="12" class="py-0">
              <v-text-field v-model="phone" label="Phone*" required />
            </v-col>
            <v-col cols="12" class="py-0">
              <v-text-field v-model="email" label="Email*" required />
            </v-col>
          </v-row>
          <small class="primary--text">*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn text @click="staffDialog = false;">Cancel</v-btn>
          <v-btn color="primary" text @click="onSaveStaff()">Save</v-btn>
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
export default {
  data() {
    return {
      staffDialog: false,
      editDialog: false,
      snackbar: false,
      id: "",
      firstName: "",
      lastName: "",
      phone: "",
      email: "",
      snackbarColor: "",
      snackbarIcon: "",
      snackbarMessage: "",
      removedStaffs: [],
      cloneStaffs: [],
      cloneStaffs1: [],
      cloneSchedule: []
    };
  },
  props: {
    dark: Array,
    user: Object,
    staffs: Object,
    schedule: Object
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    onEditStaffs() {
      this.editDialog = true;
      this.removedStaffs = [];
      this.cloneStaffs1 = JSON.parse(JSON.stringify(this.staffs));
      this.cloneSchedule = JSON.parse(JSON.stringify(this.schedule));
    },
    onCancelStaffs() {
      this.editDialog = false;
      this.staffs = this.cloneStaffs1;
      this.schedule = this.cloneSchedule;
    },
    onSaveStaffs() {
      this.editDialog = false;
      this.showSnackbar("success", "mdi-check", "Changes Saved!");

      // for (let id of this.removedStaffs) {
      //   for (let i in this.staffs) {
      //     if (id == this.staffs[i].id) {
      //       this.staffs.splice(i, 1);
      //       break;
      //     }
      //   }
      // }
    },
    onRemoveStaff(index) {
      // this.removedStaffs.push(this.staffs[index].id);

      for (let i in this.schedule) {
        for (let j in this.schedule[i]) {
          for (let k in this.schedule[i][j]) {
            if (this.schedule[i][j][k].id == this.staffs[index].id) {
              this.schedule[i][j].splice(k, 1);
            }
          }
        }
      }

      this.staffs.splice(index, 1);
    },
    onSaveStaff() {
      for (let staff of this.staffs) {
        if (staff.id == this.id) {
          this.showSnackbar("error", "mdi-alert", "ID Already Exists!");
          return;
        }
      }

      let user = {
        id: this.id,
        firstName: this.firstName,
        lastName: this.lastName,
        role: 1,
        phone: this.phone,
        email: this.email,
        color: "#2196F3",
        workingTime: [
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true]
        ]
      };
      this.staffs.push(user);
      this.staffs.sort((a, b) => {
        return a.id > b.id ? 1 : -1;
      });

      this.staffDialog = false;
      this.showSnackbar(
        "success",
        "mdi-check",
        `New Staff: ${this.firstName} ${this.lastName} Added!`
      );
    }
  }
};
</script>