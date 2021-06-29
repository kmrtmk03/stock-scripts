<template lang="pug">
  .c-pageContainer
    .c-pageContainer__inner
      h1.c-pageContainer__title Scripts Stock
      ul.links
        li.links__child
          nuxt-link(to="/linewave") linewave
        li.links__child
          nuxt-link(to="/sidescroll") sidescroll
</template>

<script>
export default {
  //スカッシュマージのテスト2
  mounted() {},
  methods: {}
}
</script>

<style lang="scss" scoped>
.c-pageContainer {
  margin: 0 auto;
  min-height: 100vh;

  &__inner {
    width: 500px;
    margin: 0 auto
  }

  &__title {
    margin: 0;
    padding: 30px 0;
  }
}

.links {
  &__child {
    margin-bottom: 20px;

    &:last-child {
      margin-bottom: 0;
    }
  }
}
</style>
