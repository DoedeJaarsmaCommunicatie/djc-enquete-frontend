<template>
    <header class="header">
      <figure>
        <a href="https://doedejaarsma.nl">
          <img :src="logo" class="logo" alt="Doede Jaarsma communicatie" />
        </a>
      </figure>
    </header>
</template>

<script>
import logo from '../assets/logo.png'

export default {
  name: 'Header',
  data: () => ({
    logo
  })
}
</script>

<style scoped lang="sass">
.logo
  max-height: 80px
  margin: 1rem auto
</style>
