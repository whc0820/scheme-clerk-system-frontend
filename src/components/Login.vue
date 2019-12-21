<template>
  <v-container
    class="d-flex justify-center align-center"
    :class="dark[0]?'dark':''"
    style="width:100%;height:100%"
  >
    <v-progress-linear absolute top indeterminate :active="isLoading" />

    <v-card
      class="pa-2 d-flex flex-column justify-center"
      width="380"
      height="380"
      style="cursor:default"
      :dark="dark[0]"
      hover
    >
      <v-card-title class="mt-5 justify-center headline">User Login</v-card-title>
      <v-card-text class="ma-0 pa-5 d-flex flex-column justify-space-around">
        <v-form>
          <v-text-field
            class="input"
            v-model="id"
            :rules="idRules"
            required
            label="ID"
            append-icon="mdi-account"
            type="text"
            outlined="ture"
          />
          <v-text-field
            class="input"
            v-model="password"
            :rules="passwordRules"
            required
            id="password"
            label="Password"
            type="password"
            append-icon="mdi-key"
            outlined="true"
          />
        </v-form>

        <v-btn class="mb-2" color="primary" @click="login()">Login</v-btn>
        <v-btn class="mb-2" color="primary" text>Forgot Password?</v-btn>
      </v-card-text>
    </v-card>

    <v-snackbar v-model="snackbar" :color="snackbarColor" bottom right>
      <div class="d-flex flex-row justify-start align-center" style="width:100%;height:100%;">
        <v-icon dark v-text="snackbarIcon" />
        <div class="mx-5">{{snackbarMessage}}</div>
      </div>
    </v-snackbar>
  </v-container>
</template>

<style>
.dark {
  background: #000;
  color: #fff;
}
</style>

<script>
export default {
  name: "login",
  data() {
    return {
      id: "",
      idRules: [v => !!v || "Id is required"],
      password: "",
      passwordRules: [v => !!v || "Password is required"],
      isLoading: false,
      snackbar: false
    };
  },
  props: {
    dark: Array,
    staffs: Array
  },
  methods: {
    showSnackbar(color, icon, message) {
      this.snackbar = true;
      this.snackbarColor = color;
      this.snackbarIcon = icon;
      this.snackbarMessage = message;
    },
    login() {
      if (this.id == "") {
        this.showSnackbar("error", "mdi-alert", "Please enter your User ID!");
      } else if (this.password == "") {
        this.showSnackbar("error", "mdi-alert", "Please enter your password!");
      } else {
        this.isLoading = true;

        let isUserExist = false;
        for (let user of this.staffs) {
          if (user.id == this.id) {
            isUserExist = true;
            break;
          }
        }
        setTimeout(() => {
          if (isUserExist) {
            this.$router.push({ path: "/main", query: { id: this.id } });
          } else {
            this.isLoading = false;
            this.showSnackbar(
              "error",
              "mdi-alert",
              "User ID or Password error!"
            );
          }
        }, 1000);
      }
    }
  }
};
</script>