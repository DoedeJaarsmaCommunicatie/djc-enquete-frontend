<template>
  <div id="snow">
    <header class="header">
      <figure>
        <img :src="logo" class="logo" alt="Doede Jaarsma communicatie" @click="reload" />
      </figure>
    </header>
  </div>
</template>

<script>
import logo from '../assets/logo.png'

export default {
  name: 'Header',
  data: () => ({
    logo
  }),
  methods: {
    reload () {
      window.location.reload()
    }
  }
}
</script>

<style scoped lang="sass">
.logo
  max-height: 80px
  margin: 1rem auto

@media screen and (max-width: 769px)
  .header
    @apply bg-primary-500
    position: fixed
    top: 0
    left: 0
    right: 0
    z-index: 9999399

  .logo
    height: 60px
</style>
