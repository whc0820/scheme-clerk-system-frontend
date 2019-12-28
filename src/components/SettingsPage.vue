<template>
  <v-container>
    <v-row align-content="start">
      <v-col class="mx-2" cols="12">
        <v-row justify="start">
          <div class="display-1">Settings</div>
        </v-row>
      </v-col>

      <v-col cols="12" class="mt-1 pa-0">
        <v-tabs
          background-color="transparent"
          v-model="selectedTab"
          :dark="dark[0]"
          color="primary"
        >
          <v-tab v-for="(tab, i) in tabs" :key="i" v-text="tab" />
        </v-tabs>
        <v-divider :dark="dark[0]" />
      </v-col>

      <v-col class="col-12" lg="8">
        <v-row>
          <v-col cols="12" v-if="selectedTab == 0">
            <div class="mb-4 text-left title">Profile Picture</div>
            <v-card class="mb-5 py-2" :dark="dark[0]">
              <div class="d-flex align-center ms-5">
                <v-avatar size="72">
                  <v-icon size="72" color="primary">mdi-account-circle</v-icon>
                </v-avatar>
                <v-card-text>
                  <div class="d-flex flex-column justify-space-around">
                    <v-btn class="mb-2" color="primary" width="200" outlined>Add Profile Picture</v-btn>
                    <div class="text-left">Must be JPEG, PNG, or GIF and cannot exceed 10MB.</div>
                  </div>
                </v-card-text>
              </div>
            </v-card>

            <div class="mb-2 text-left title">Profile Settings</div>
            <div class="mb-2 d-flex justify-space-between align-center">
              <div class="text-left body-2">Change personal details for your account</div>
              <v-btn color="primary" @click="onEditProfile()">Edit</v-btn>
            </div>
            <v-card width="100%" :dark="dark[0]">
              <v-card-text>
                <v-row>
                  <v-col cols="12" class="px-5 d-flex flex-row justify-start align-center">
                    <v-icon>mdi-account</v-icon>

                    <div
                      class="ms-10 text-left body-1"
                      v-text="`${user.firstName} ${user.lastName}`"
                    ></div>
                  </v-col>
                  <v-col cols="12" class="py-0">
                    <v-divider inset />
                  </v-col>

                  <v-col cols="12" class="px-5 d-flex flex-row justify-start align-center">
                    <v-icon>mdi-phone</v-icon>

                    <div class="ms-10 text-left body-1" v-text="user.phone"></div>
                  </v-col>

                  <v-col cols="12" class="py-0">
                    <v-divider inset />
                  </v-col>

                  <v-col cols="12" class="px-5 d-flex flex-row justify-start align-center">
                    <v-icon>mdi-email</v-icon>

                    <div class="ms-10 text-left body-1" v-text="user.email"></div>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="12" v-else-if="selectedTab == 1">
            <div class="mb-4 text-left title">Dark Mode</div>
            <v-card class="mb-5" :dark="dark[0]">
              <v-card-text class="d-flex flex-row justify-space-between align-center">
                <div class="text-left body-2">Enable dark mode</div>
                <v-switch color="primary" v-model="dark[0]" />
              </v-card-text>
            </v-card>

            <div class="mb-2 text-left title">Theme Color</div>
            <div class="mb-2 text-left body-2">Select your favorite color for the theme</div>
            <ColorPicker :dark="dark[0]" :user="user" />
          </v-col>
          <v-col cols="12" v-else-if="selectedTab == 2">
            <div class="mb-2 text-left title">Security</div>
            <div class="mb-2 text-left body-2">Keep your account safe</div>

            <v-card :dark="dark[0]">
              <v-card-text class="d-flex justify-start align-center">
                <div class="me-10 text-left">Password</div>
                <div
                  class="ma-0 pa-1 caption primary--text underline"
                  style="cursor: pointer"
                  @click="onClickChangePassword"
                >Change password.</div>

                <div class="caption">Improve your security with a strong password.</div>
              </v-card-text>
            </v-card>
            <v-dialog width="500" v-model="passwordDialog">
              <v-card :dark="dark[0]">
                <v-card-title>
                  <span class="headline primary--text">Change Password</span>
                </v-card-title>
                <v-card-text>
                  <v-row>
                    <v-col cols="12" class="py-0">
                      <v-text-field
                        type="password"
                        v-model="newPassword"
                        label="New Password*"
                        required
                      />
                    </v-col>
                    <v-col cols="12" class="py-0">
                      <v-text-field
                        type="password"
                        v-model="verifyPassword"
                        label="Verify Password*"
                        required
                      />
                    </v-col>
                  </v-row>
                  <small class="primary--text">*indicates required field</small>
                </v-card-text>
                <v-card-actions>
                  <v-spacer />
                  <v-btn text @click="passwordDialog = false">Cancel</v-btn>
                  <v-btn color="primary" text @click="onClickSavePassword">Save</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-col>
        </v-row>
      </v-col>
    </v-row>

    <v-snackbar v-model="snackbar" :color="snackbarColor" bottom right>
      <div class="d-flex flex-row justify-start align-center" style="width:100%;height:100%;">
        <v-icon dark v-text="snackbarIcon" />
        <div class="mx-5">{{snackbarMessage}}</div>
      </div>
    </v-snackbar>

    <v-dialog width="500" v-model="profileDialog">
      <v-card :dark="dark[0]">
        <v-card-title>
          <span class="headline primary--text">Profile</span>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col class="py-0" cols="6">
              <v-text-field type="text" label="First Name*" v-model="firstName" />
            </v-col>
            <v-col class="py-0" cols="6">
              <v-text-field type="text" label="Last Name*" v-model="lastName" />
            </v-col>
            <v-col class="py-0" cols="12">
              <v-text-field type="text" label="Phone*" v-model="phone" />
            </v-col>
            <v-col class="py-0" cols="12">
              <v-text-field type="text" label="Email*" v-model="email" />
            </v-col>
          </v-row>

          <small class="primary--text">*indicates required field</small>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="onCancelProfile()">Cancel</v-btn>
          <v-btn text color="primary" @click="onSaveProfile()">Save</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<style>
.underline:hover {
  text-decoration: underline;
}
</style>

<script>
import ColorPicker from "./ColorPicker";

export default {
  data() {
    return {
      selectedTab: 0,
      tabs: ["Profile", "Preference", "Security"],
      snackbar: false,
      snackbarColor: "",
      snackbarIcon: "",
      snackbarMessage: "",
      isLoading: true,
      profileDialog: false,
      passwordDialog: false,
      firstName: this.user.firstName,
      lastName: this.user.lastName,
      phone: this.user.phone,
      email: this.user.email,
      cloneFirstName: "",
      cloneLastName: "",
      clonePhone: "",
      cloneEmail: "",
      newPassword: "",
      verifyPassword: ""
    };
  },
  props: {
    dark: Array,
    user: Object
  },
  components: {
    ColorPicker
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    onEditProfile() {
      this.profileDialog = true;
      this.cloneFirstName = this.user.firstName;
      this.cloneLastName = this.user.lastName;
      this.clonePhone = this.user.phone;
      this.cloneEmail = this.user.email;
    },
    onCancelProfile() {
      this.profileDialog = false;
      this.firstName = this.cloneFirstName;
      this.lastName = this.cloneLastName;
      this.phone = this.clonePhone;
      this.email = this.cloneEmail;
    },
    onSaveProfile() {
      this.showSnackbar("success", "mdi-check", "Profile Saved!");
      this.profileDialog = false;
      this.user.firstName = this.firstName;
      this.user.lastName = this.lastName;
      this.user.phone = this.phone;
      this.user.email = this.email;
    },
    onClickChangePassword() {
      this.passwordDialog = true;
    },
    onClickSavePassword() {
      if (this.newPassword != this.verifyPassword) {
        this.showSnackbar("error", "mdi-error", "Please verify your password!");
      } else {
        this.showSnackbar("success", "mdi-check", "Password Changed!");
        this.passwordDialog = false;
      }
    }
  }
};
</script>