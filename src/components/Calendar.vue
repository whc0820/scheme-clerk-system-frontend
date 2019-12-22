<template>
  <v-card :dark="dark[0]">
    <v-card-text>
      <v-calendar type="month" color="primary" :events="events" :event-color="getEventColor"/>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      events: [
        {
          name: "Christmas",
          start: "2019-12-25",
          color: "red"
        },
        {
          name: "Final Exam",
          start: "2020-01-06",
          end: "2020-01-11",
          color: "red"
        }
      ]
    };
  },
  props: {
    dark: Array
  },
  methods: {
    getEventColor(event) {
      return event.color;
    }
  }
};
</script>