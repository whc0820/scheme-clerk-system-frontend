<template>
  <v-card :dark="dark[0]">
    <v-card-text>
      <v-calendar type="month" color="primary" />
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    dark: Array
  }
};
</script>