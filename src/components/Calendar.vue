<template>
  <v-card :dark="dark[0]">
    <v-card-text>
      <v-sheet height="300">
        <v-calendar
          color="primary"
          :events="events"
          :event-color="getEventColor"
          :event-more="more"
        />
      </v-sheet>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {
      more: false,
    };
  },
  props: {
    dark: Array,
    events: Array,
  },
  methods: {
    getEventColor(event) {
      return event.color;
    },
  },
};
</script>