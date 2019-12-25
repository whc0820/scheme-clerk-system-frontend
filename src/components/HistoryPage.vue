<template>
  <v-container class="px-5">
    <v-row>
      <v-col class="d-flex flex-column" cols="12">
        <div class="mb-6 display-1">History</div>
        <v-divider class="mb-6" :dark="dark[0]" />
        <v-col cols="6">
          <v-timeline :dark="dark[0]" dense>
            <v-timeline-item v-for="(item, i) in history.slice().reverse()" :key="i" :color="item.color" small right>
              <div class="py-2 d-flex flex-row align-center">
                <span class="body-2 me-10" v-text="item.time" />
                <span class="body-2" v-text="item.content" />

                <v-spacer />
                <span class="body-2" v-text="`${item.firstName} ${item.lastName}`" />
              </div>
              <v-divider />
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
    dark: Object,
    user: Object,
    history: Object
  }
};
</script>

<style>
.underline:hover {
  text-decoration: underline;
}
</style>