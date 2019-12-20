<template>
  <v-container class="d-flex justify-center align-center" :class="dark[0]?'dark':''" style="width:100%;height:100%">
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
      isLoading: false
    };
  },
  props: {
    dark: Boolean
  },
  methods: {
    login() {
      this.isLoading = !this.isLoading;
      setTimeout(() => {
        this.$router.push({ path: "/main", query: { id: this.id } });
      }, 1000);
    }
  }
};
</script>