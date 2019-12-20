<template>
  <v-container class="px-5">
    <v-row>
      <v-col cols="12">
        <div class="display-1">About</div>
      </v-col>
      <v-col cols="12">
        <v-divider :dark="dark[0]"/>
      </v-col>

      <v-col cols="12">
        <div class="headline">Scheme Clerk System</div>
        <div class="px-5 my-2">- This system is used for Interaction Design Final Project</div>
        <div class="px-5 my-2 d-flex flex-row justify-start align-center">
          <div class="body-1">- Designed by</div>
          <div class="ms-2 body-1 primary--text underline">Jason Chen 陳威翰 1086035</div>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    dark: Array
  }
};
</script>

<style>
.underline:hover {
  text-decoration: underline;
}
</style>