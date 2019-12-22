<template>
  <v-color-picker
    v-model="color"
    width="100%"
    swatches-max-height="250"
    :dark="dark"
    hide-canvas
    hide-mode-switch
    hide-inputs
    show-swatches
  />
</template>

<script>
export default {
  data() {
    return {
      types: ["hex", "hexa", "rgba", "hsla", "hsva"],
      type: "hex",
      hex: this.$vuetify.theme.themes.light.primary
      // hexa: "#FF00FFFF",
      // rgba: { r: 255, g: 0, b: 255, a: 1 },
      // hsla: { h: 300, s: 1, l: 0.5, a: 1 },
      // hsva: { h: 300, s: 1, v: 1, a: 1 }
    };
  },
  props: {
    dark: Boolean,
    user: Object
  },
  computed: {
    color: {
      get() {
        return this[this.type];
      },
      set(v) {
        this[this.type] = v;
        this.$vuetify.theme.themes.light.primary = v;
        this.$vuetify.theme.themes.dark.primary = v;
        this.user.color = this.hex;
      }
    }
  }
};
</script>