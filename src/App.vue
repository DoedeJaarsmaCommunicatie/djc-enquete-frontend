<template>
  <div id="app" v-if="$store.state.user">
    <Header />
    <transition-group>
      <section v-if="!started" key="button">
        <button class="btn primary" @click="started = 'questions'">Beginnen</button>
      </section>
      <section v-else-if="started === 'questions'" key="questionnaire"><Questionnaire @finished="started = 'thanks'" /></section>
      <section v-else key="thanks"><Thanks /></section>
    </transition-group>
  </div>
</template>

<script>
import Questionnaire from '@/views/Questionnaire'
import Thanks from '@/views/Thanks'
import ky from 'ky'
import { baseUri } from '@/config'
import Header from './components/Header'

export default {
  name: 'app',
  components: { Header, Thanks, Questionnaire },
  data: () => ({
    started: 'questions'
  }),
  async mounted () {
    const id = new URLSearchParams(window.location.search).get('gebruiker')
    if (id) {
      const user = await ky.get(`${baseUri}/user/${id}`).json()
      this.$store.commit('login', { user })
    }
  }
}
</script>

<style lang="scss" src="./assets/tailwind.scss" />
