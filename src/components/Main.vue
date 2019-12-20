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
      <StaffsPage
        :dark="dark"
        :user="user"
        :staffs="staffs"
        v-else-if="selectedIndex == 2"
      />
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

let mStaffs = [
  {
    id: "s0001",
    firstName: "Jason",
    lastName: "Chen",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "red lighten-2",
    workingTime: [
      [true, true, true],
      [true, true, true],
      [true, true, true],
      [true, true, true],
      [true, true, true],
      [true, true, true],
      [true, true, true]
    ]
  },
  {
    id: "s0002",
    firstName: "Scot",
    lastName: "Liou",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "pink lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0003",
    firstName: "Janelle",
    lastName: "Wu",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "purple lighten-2",
    workingTime: [
      [true, false, true],
      [false, true, false],
      [true, false, true],
      [false, true, false],
      [true, false, true],
      [false, true, false],
      [true, false, true]
    ]
  },
  {
    id: "s0004",
    firstName: "Angel",
    lastName: "Tsao",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "deep-purple lighten-2",
    workingTime: [
      [false, true, false],
      [true, false, true],
      [false, true, false],
      [true, false, true],
      [false, true, false],
      [true, false, true],
      [false, true, false]
    ]
  },
  {
    id: "s0005",
    firstName: "Tony",
    lastName: "Lin",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "indigo lighten-2",
    workingTime: [
      [true, true, true],
      [false, false, false],
      [true, true, true],
      [false, false, false],
      [true, true, true],
      [false, false, false],
      [true, true, true]
    ]
  },
  {
    id: "s0006",
    firstName: "Ted",
    lastName: "Lin",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "blue lighten-2",
    workingTime: [
      [false, false, false],
      [true, true, true],
      [false, false, false],
      [true, true, true],
      [false, false, false],
      [true, true, true],
      [false, false, false]
    ]
  },
  {
    id: "s0007",
    firstName: "Edward",
    lastName: "Huang",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "light-blue lighten-2",
    workingTime: [
      [true, false, true],
      [true, false, true],
      [true, false, true],
      [true, false, true],
      [true, false, true],
      [true, false, true],
      [true, false, true]
    ]
  },
  {
    id: "s0008",
    firstName: "Nyoto",
    lastName: "Art",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "cyan lighten-2",
    workingTime: [
      [false, true, false],
      [false, true, false],
      [false, true, false],
      [false, true, false],
      [false, true, false],
      [false, true, false],
      [false, true, false]
    ]
  },
  {
    id: "s0009",
    firstName: "Nasin",
    lastName: "Lin",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "teal lighten-2",
    workingTime: [
      [true, false, false],
      [false, true, false],
      [false, false, true],
      [true, false, false],
      [false, true, false],
      [false, false, true],
      [true, false, false]
    ]
  },
  {
    id: "s0010",
    firstName: "Fermi",
    lastName: "Chen",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "green lighten-2",
    workingTime: [
      [false, false, true],
      [false, true, false],
      [true, false, false],
      [false, false, true],
      [false, true, false],
      [true, false, false],
      [false, false, true]
    ]
  },
  {
    id: "s0011",
    firstName: "Jessie",
    lastName: "Kao",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "light-green lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0012",
    firstName: "Tess",
    lastName: "Wu",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "lime lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0013",
    firstName: "Jimmy",
    lastName: "U",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "yellow lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0014",
    firstName: "Richard",
    lastName: "Yiou",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "amber lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0015",
    firstName: "York",
    lastName: "Liou",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "orange lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0016",
    firstName: "Mike",
    lastName: "Yung",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "deep-orange lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0017",
    firstName: "Jacky",
    lastName: "Chen",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "brown lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  },
  {
    id: "s0018",
    firstName: "Joyce",
    lastName: "Chang",
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "blue-grey lighten-2",
    workingTime: [
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false],
      [false, false, false]
    ]
  }
];

let mSchedule = [
  [
    [mStaffs[0], mStaffs[1]],
    [mStaffs[2], mStaffs[3]],
    [mStaffs[4], mStaffs[5]]
  ],
  [
    [mStaffs[6], mStaffs[7]],
    [mStaffs[8], mStaffs[9]],
    [mStaffs[10], mStaffs[11]]
  ],
  [
    [mStaffs[12], mStaffs[13]],
    [mStaffs[14], mStaffs[15]],
    [mStaffs[16], mStaffs[17]]
  ],
  [
    [mStaffs[0], mStaffs[1]],
    [mStaffs[2], mStaffs[3]],
    [mStaffs[4], mStaffs[5]]
  ],
  [
    [mStaffs[6], mStaffs[7]],
    [mStaffs[8], mStaffs[9]],
    [mStaffs[10], mStaffs[11]]
  ],
  [
    [mStaffs[12], mStaffs[13]],
    [mStaffs[14], mStaffs[15]],
    [mStaffs[16], mStaffs[17]]
  ],
  [[mStaffs[0], mStaffs[1]], [mStaffs[2], mStaffs[3]], [mStaffs[4], mStaffs[5]]]
];

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
      },
      schedule: mSchedule,
      // schedule: [
      //   [[], [], []],
      //   [[], [], []],
      //   [[], [], []],
      //   [[], [], []],
      //   [[], [], []],
      //   [[], [], []],
      //   [[], [], []]
      // ],
      staffs: mStaffs
    };
  },
  props: {
    dark: Array
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
    if (this.$route.query.id == "s0000") {
      this.user = {
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
      };
    } else {
      this.user = {
        id: this.$route.query.id,
        firstName: "Staff",
        lastName: "A",
        role: 1,
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
      };
    }
  }
};
</script>