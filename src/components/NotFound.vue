<template>
  <v-sheet color="primary" width="100%" height="100%" tile style="position:relative">
    <div
      class="pa-10 font-weight-thin white--text"
      style="position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);font-size:192px;border:5px solid #fff;"
    >Oops</div>
    <div
      class="font-weight-bold white--text display-3"
      style="position:absolute;top:30%;right:100px;transform:translate(-50%,-50%) scaleX(-1)"
    >404</div>
    <div
      class="font-weight-thin white--text display-4 text-uppercase"
      style="position:absolute;top:50%;left:100px;transform:translate(-50%, -50%) rotate(90deg) scaleY(-1);"
    >Page Not Found</div>
    <div
      class="font-weight-light white--text title text-uppercase"
      style="position:absolute;bottom:50px;left:50%;transform:translate(-50%, -50%) rotate(180deg) scaleX(-1);"
    >The page you were looking for doesnt exist</div>
  </v-sheet>
</template>

<script>
export default {};
</script>

<style>
</style>