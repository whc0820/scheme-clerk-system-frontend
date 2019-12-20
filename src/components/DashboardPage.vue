<template>
  <v-container style="width:100%;height:100%">
    <v-row align-content="start" class="px-5">
      <v-col cols="12">
        <v-row justify="start">
          <div class="display-1">Dashboard</div>
        </v-row>
      </v-col>

      <v-col class="mx-0 px-0" cols="12">
        <v-divider :dark="dark[0]" />
      </v-col>
      <v-col cols="12" lg="6">
        <Calendar :dark="dark" />
      </v-col>

      <v-col cols="12" xl="10">
        <Schedule :dark="dark" :isEditing="false" :schedule="schedule" />
      </v-col>
    </v-row>
  </v-container>
</template>

<style>
.dark {
  /* background: #000; */
  color: #fff;
}
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
    dark: Object,
    schedule: Object
  },

  components: {
    Calendar,
    Schedule
  },
  methods: {}
};
</script>