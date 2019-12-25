<template>
  <v-container class="px-5">
    <v-row>
      <v-col class="d-flex flex-column" cols="12">
        <div class="mb-6 display-1">History</div>
        <v-divider class="mb-6" :dark="dark[0]" />
        <v-col class="py-0" cols="6">
          <v-timeline :dark="dark[0]">
            <v-timeline-item v-for="n in 5" :key="n" color="primary" small>
              <template v-slot:opposite>
                <span>Tus eu perfecto</span>
              </template>
              <v-card>
                <v-card-text>
                  <span class="headline">Lorem ipsum</span>
                  <div
                    class="py-4"
                  >Lorem ipsum dolor sit amet, no nam oblique veritus. Commune scaevola imperdiet nec ut, sed euismod convenire principes at. Est et nobis iisque percipit, an vim zril disputando voluptatibus, vix an salutandi sententiae.</div>
                </v-card-text>
              </v-card>
            </v-timeline-item>
          </v-timeline>
        </v-col>
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