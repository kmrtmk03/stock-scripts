<template lang="pug">
  .c-linkHome
    .c-linkHome__inner
      nuxt-link(to="/").c-linkHome__button
</template>

<script>
export default {
  mounted() {},
  methods: {}
}
</script>

<style lang="scss" scoped>
  .c-linkHome {
    $_size: 50px;

    position: fixed;
    top: 30px;
    left: 30px;
    &__button {
      display: block;
      width: $_size;
      height: $_size;
      border-radius: 50%;
      background-color: $color_main;
    }
  }
</style>
