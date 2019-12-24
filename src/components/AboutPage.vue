<template>
  <v-container class="px-5">
    <v-row>
      <v-col class="d-flex flex-column" cols="12">
        <div class="mb-6 display-1">About</div>
        <v-divider class="mb-6" :dark="dark[0]" />
        <span class="headline">Scheme Clerk System</span>
      </v-col>
      <v-col cols="6">
        <div class="px-5 d-flex justify-start">
          <v-timeline :dark="dark[0]">
            <v-timeline-item v-for="n in 5" :key="n" color="primary">
              <template v-slot:opposite>
                <span>Tus eu perfecto</span>
              </template>
              <v-card class="elevation-2">
                <v-card-title class="headline">Lorem ipsum</v-card-title>
                <v-card-text>Lorem ipsum dolor sit amet, no nam oblique veritus. Commune scaevola imperdiet nec ut, sed euismod convenire principes at. Est et nobis iisque percipit, an vim zril disputando voluptatibus, vix an salutandi sententiae.</v-card-text>
              </v-card>
            </v-timeline-item>
          </v-timeline>
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