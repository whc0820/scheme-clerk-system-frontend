<template>
  <v-container
    class="d-flex justify-center align-center"
    :class="dark[0]?'dark':''"
    style="width:100%;height:100%"
  >
    <v-progress-linear absolute top indeterminate :active="isLoading" />

    <v-tooltip :dark="dark[0]" color="transparent" transition="fade-transition" left>
      <template v-slot:activator="{on}">
        <v-btn
          v-on="on"
          :color="dark[0]?'white':'grey darken-2'"
          style="cursor:default"
          absolute
          top
          right
          icon
        >
          <v-icon>mdi-information-outline</v-icon>
        </v-btn>
      </template>
      <span
        class="body-2"
        :class="dark[0]?'white--text':'primary--text'"
      >Interaction Design Final Project - 1086035 Jason Chen</span>
    </v-tooltip>

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

    <v-snackbar v-model="snackbar" :color="snackbarColor" timeout="2000" bottom right>
      <div class="d-flex flex-row justify-start align-center" style="width:100%;height:100%;">
        <v-icon dark v-text="snackbarIcon" />
        <div class="mx-5">{{snackbarMessage}}</div>
      </div>
    </v-snackbar>
  </v-container>
</template>

<style>
.dark {
  background: #fff;
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
    users: Array
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
        this.showSnackbar("error", "mdi-alert", "Please Enter Your ID!");
      } else if (this.password == "") {
        this.showSnackbar("error", "mdi-alert", "Please Enter Your Password!");
      } else {
        this.isLoading = true;

        let isUserExist = false;
        for (let user of this.users) {
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
            this.showSnackbar("error", "mdi-alert", "ID or Password Error!");
          }
        }, 1000);
      }
    }
  }
};
</script>