<template lang="pug">
  .header
    .inner-margin
      nuxt-link.header__logo(to='/')
        img(src="~/assets/images/sushi/SU-SHI.svg")
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.header {
  position: absolute;
  z-index: 99;
  top: 0;
  left: 0;
  height: 80px;
  width: 100%;
  background-color: transparent;
}
</style>
