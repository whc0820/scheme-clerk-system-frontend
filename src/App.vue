<template>
  <div id="app">
    <v-app>
      <v-content :class="dark[0]?'dark':''">
        <router-view
          :dark="dark"
          :schedule="schedule"
          :users="users"
          :events="events"
          :history="history"
          style="width:100%;height:100%;"
        />
      </v-content>
    </v-app>
  </div>
</template>

<style>
.dark {
  background: #000;
  color: #fff;
}
</style>

<script>
let mUsers = [
  {
    id: "s0000",
    firstName: "Super",
    lastName: "Admin",
    role: 0,
    phone: "0912345678",
    email: "vuetify@example.com",
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
  },
  {
    id: "s0001",
    firstName: "Jason",
    lastName: "Chen",
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#E57373",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#F06292",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#BA68C8",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#9575CD",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#7986CB",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#64B5F6",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#4FC3F7",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#4DD0E1",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#4DB6AC",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#81C784",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#AED581",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#DCE775",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#FFF176",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#FFD54F",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#FFB74D",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#FF8A65",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#A1887F",
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
    role: 1,
    phone: "0912345678",
    email: "vuetify@example.com",
    color: "#90A4AE",
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
  [[mUsers[1], mUsers[2]], [mUsers[3], mUsers[4]], [mUsers[5], mUsers[6]]],
  [[mUsers[7], mUsers[8]], [mUsers[9], mUsers[10]], [mUsers[11], mUsers[12]]],
  [
    [mUsers[13], mUsers[14]],
    [mUsers[15], mUsers[16]],
    [mUsers[17], mUsers[18]]
  ],
  [[mUsers[1], mUsers[2]], [mUsers[3], mUsers[4]], [mUsers[5], mUsers[6]]],
  [[mUsers[7], mUsers[8]], [mUsers[9], mUsers[10]], [mUsers[11], mUsers[12]]],
  [
    [mUsers[13], mUsers[14]],
    [mUsers[15], mUsers[16]],
    [mUsers[17], mUsers[18]]
  ],
  [[mUsers[1], mUsers[2]], [mUsers[3], mUsers[4]], [mUsers[5], mUsers[6]]]
];

let mHistory = [];

export default {
  data() {
    return {
      dark: [false],
      users: mUsers,
      schedule: mSchedule,
      history: mHistory,
      events: [
        {
          name: "Weekly Meeting",
          start: "2019-12-02",
          end: "2019-12-02",
          color: "blue lighten-2"
        },
        {
          name: "Weekly Meeting",
          start: "2019-12-09",
          end: "2019-12-09",
          color: "blue lighten-2"
        },
        {
          name: "Weekly Meeting",
          start: "2019-12-16",
          end: "2019-12-16",
          color: "blue lighten-2"
        },
        {
          name: "Weekly Meeting",
          start: "2019-12-23",
          end: "2019-12-23",
          color: "blue lighten-2"
        },
        {
          name: "Weekly Meeting",
          start: "2019-12-30",
          end: "2019-12-30",
          color: "blue lighten-2"
        },
        {
          name: "Christmas Holiday",
          start: "2019-12-25",
          end: "2019-12-26",
          color: "pink lighten-2"
        },
        {
          name: "New Year Holiday",
          start: "2020-01-01",
          end: "2020-01-04",
          color: "green lighten-2"
        }
      ]
    };
  }
};
</script>