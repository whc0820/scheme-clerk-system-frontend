<template>
  <v-card :dark="dark[0]">
    <v-card-text>
      <v-calendar color="primary" :events="events" :event-color="getEventColor" event-more="false" />
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    dark: Array,
    events: Array
  },
  methods: {
    getEventColor(event) {
      return event.color;
    }
  }
};
</script>