<template>
  <div id="app">
    <transition-group>
      <section v-if="!started" key="button">
        <button class="btn primary" @click="started = true">Beginnen</button>
      </section>
      <section v-else key="questionnaire">
        <Questionnaire/>
      </section>
    </transition-group>
  </div>
</template>

<script>
import Questionnaire from '@/views/Questionnaire'
export default {
  name: 'app',
  components: { Questionnaire },
  data: () => ({
    started: false
  })
}
</script>

<style lang="scss" src="./assets/tailwind.scss" />
