<template>
  <div id="app">
    <v-app>
      <v-content :class="dark[0]?'dark':''">
        <router-view :dark="dark" style="width:100%;height:100%;" />
      </v-content>
    </v-app>
  </div>
</template>

<style>
.dark {
  background: #000;
  color: #fff;
}
</style>

<script>
export default {
  data() {
    return {
      dark: [false]
    };
  }
};
</script>