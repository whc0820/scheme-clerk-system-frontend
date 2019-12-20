<template>
  <v-container>
    <v-row align-content="start" class="px-5">
      <v-col cols="12">
        <v-row justify="space-between">
          <div class="display-1">Staffs</div>
          <v-dialog width="500" v-model="dialog" v-if="user.role == 0">
            <template v-slot:activator="{on}">
              <v-btn fab dark color="primary" v-on="on">
                <v-icon>mdi-plus</v-icon>
              </v-btn>
            </template>
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
                <v-btn text @click="dialog = false">Cancel</v-btn>
                <v-btn color="primary" text @click="onClickSave()">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </v-col>

      <v-col class="mx-0 px-0" cols="12">
        <v-divider :dark="dark[0]" />
      </v-col>
      <v-col cols="12" lg="8">
        <v-expansion-panels :dark="dark[0]">
          <v-expansion-panel v-for="(staff,i) in staffs" :key="i">
            <v-expansion-panel-header class="ma-0 px-4 py-4">
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
      dialog: false,
      snackbar: false,
      id: "",
      firstName: "",
      lastName: "",
      phone: "",
      email: "",
      snackbarColor: "",
      snackbarIcon: "",
      snackbarMessage: ""
    };
  },
  props: {
    dark: Array,
    user: Object,
    staffs: Object
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    onClickSave() {
      for (let staff of this.staffs) {
        if (staff.id == this.id) {
          this.showSnackbar("error", "mdi-alert", "ID Already Exists!");
          return;
        }
      }

      let user = {
        firstName: this.firstName,
        lastName: this.lastName,
        id: this.id,
        phone: this.phone,
        email: this.email,
        color: "blue lighten-2",
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
      this.dialog = false;
      this.showSnackbar(
        "success",
        "mdi-check",
        `New Staff: ${this.firstName} ${this.lastName} Added!`
      );
    }
  },
  created() {
    // alert(JSON.stringify(this.dark));
  }
};
</script>