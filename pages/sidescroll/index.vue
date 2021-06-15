<template lang="pug">
  .c-sidescroll
    LinkHome
    SideScroll
</template>

<script>
export default {
  mounted() {
    this.ScrollNotArrow()
  },

  methods: {
    NoScroll(e) {
      e.preventDefault()
    },

    ScrollNotArrow() {
      document.addEventListener('touchmove', this.NoScroll, { passive: false });
      document.addEventListener('mousewheel', this.NoScroll, { passive: false });
    }
  }

}
</script>

<style lang="scss" scoped>
</style>
