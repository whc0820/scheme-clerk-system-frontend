<template>
  <v-content class="pa-0 ma-0" :class="dark[0]?'dark':''" style="width:100%;height:100%">
    <v-navigation-drawer v-model="drawer" :dark="dark[0]" app clipped expand-on-hover>
      <v-list nav dense flat>
        <v-list-item>
          <v-list-item-avatar>
            <v-avatar color="primary">
              <span class="white--text headline" v-text="user.firstName[0] + user.lastName[0]" />
            </v-avatar>
          </v-list-item-avatar>
        </v-list-item>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title class="mb-2 title" v-text="`${user.firstName} ${user.lastName}`"></v-list-item-title>
            <v-list-item-subtitle v-text="user.role==0?'Manager':'Staff'" />
          </v-list-item-content>
        </v-list-item>
        <v-divider class="mb-5" />
        <v-list-item-group color="primary" v-model="selectedIndex" mandatory>
          <v-list-item v-for="(item, i) in drawerItems" :key="i" @click="onDrawerItemClick(i)">
            <v-list-item-icon>
              <v-icon v-text="item.icon" />
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left" v-text="item.text" />
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app :dark="dark[0]" clipped-left :color="dark[0]?'grey-darken-4':'grey-lighten-4'">
      <v-app-bar-nav-icon @click="drawer = !drawer" />
    </v-app-bar>

    <v-container class="pa-0 ma-0" style="width:100%; height:100%;">
      <DashboardPage :dark="dark" :schedule="schedule" v-if="selectedIndex == 0" />
      <SchedulePage
        :dark="dark"
        :user="user"
        :schedule="schedule"
        :staffs="staffs"
        v-else-if="selectedIndex == 1"
      />
      <StaffsPage :dark="dark" :user="user" :staffs="staffs" v-else-if="selectedIndex == 2" />
      <SettingsPage :dark="dark" :user="user" v-else-if="selectedIndex == 3" />
      <AboutPage :dark="dark" v-else-if="selectedIndex == 4" />
    </v-container>
  </v-content>
</template>

<style>
.dark {
  background: #000;
  color: #fff;
}
</style>

<script>
import DashboardPage from "./DashboardPage";
import SchedulePage from "./SchedulePage";
import StaffsPage from "./StaffsPage";
import SettingsPage from "./SettingsPage";
import AboutPage from "./AboutPage";

export default {
  data() {
    return {
      drawer: null,
      selectedIndex: 0,
      drawerItems: [
        {
          text: "Dashboard",
          icon: "mdi-view-dashboard"
        },
        {
          text: "Schedule",
          icon: "mdi-calendar"
        },
        {
          text: "Staffs",
          icon: "mdi-account-multiple"
        },
        {
          text: "Settings",
          icon: "mdi-settings"
        },
        {
          text: "About",
          icon: "mdi-information"
        },
        {
          text: "Logout",
          icon: "mdi-arrow-right-circle"
        }
      ],
      user: {
        id: "s0000",
        firstName: "Super",
        lastName: "Admin",
        role: 0,
        phone: "0912345678",
        email: "vuetify@example.com",
        workingTime: [
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true],
          [true, true, true]
        ],
        color: "red lighten-2"
      }
    };
  },
  props: {
    dark: Array,
    schedule: Object,
    staffs: Object
  },
  components: {
    DashboardPage,
    SchedulePage,
    StaffsPage,
    SettingsPage,
    AboutPage
  },
  methods: {
    onDrawerItemClick(index) {
      if (index == 5) {
        this.$router.push({ path: "/login" });
      }
    }
  },
  created() {
    for (let user of this.staffs) {
      if (user.id == this.$route.query.id) {
        this.user = user;
        break;
      }
    }

    this.$vuetify.theme.themes.light.primary = this.user.color;
    this.$vuetify.theme.themes.dark.primary = this.user.color;
  }
};
</script>