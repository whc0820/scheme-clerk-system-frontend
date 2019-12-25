<template>
  <v-container style="width:100%;height:100%;">
    <v-row align-content="start">
      <v-col class="mx-2" cols="12">
        <v-row justify="start">
          <div class="display-1">Dashboard</div>
        </v-row>
      </v-col>

      <v-col class="mx-0 px-0" cols="12">
        <v-divider :dark="dark[0]" />
      </v-col>
      <v-col cols="12" lg="10" xl="6">
        <Calendar :dark="dark" :events="events" />
      </v-col>

      <v-col cols="12" xl="10">
        <Schedule :dark="dark" :isEditing="false" :schedule="schedule" />
      </v-col>
    </v-row>
  </v-container>
</template>

<style>
</style>

<script>
import Calendar from "./Calendar";
import Schedule from "./Schedule";

export default {
  data() {
    return {
      picker: new Date().toISOString().substr(0, 10)
    };
  },
  props: {
    dark: Array,
    schedule: Array,
    events: Array
  },

  components: {
    Calendar,
    Schedule
  },
  methods: {}
};
</script>