<template lang="pug">
  .container
    Header
    nuxt
</template>

<script>
import Header from '~/components/Header'

export default {
  components: {
    Header
  },
  mounted() {
    window.addEventListener('resize', () => {
      const vh = window.innerHeight * 0.01
      document.documentElement.style.setProperty('--vh', `${vh}px`)
    })
  }
}
</script>

<style lang="scss">
.container {
  min-height: calc(var(--vh, 1vh) * 100);
  background-color: $color-lightgray;
}
</style>
