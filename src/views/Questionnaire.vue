<template>
  <main id="main">
    <section v-for="(question, key) in questions"
             :key="question.id"
             style="height: 100%; padding-bottom: 12rem"
             v-show="key === steps.current">
      <Question :question="question" @picked="answerChosen" />
    </section>
  </main>
</template>

<script>
import { baseUri } from '@/config'
import ky from 'ky'
import Question from '@/components/Question'

export default {
  name: 'Questionnaire',
  components: { Question },
  data: () => ({
    questions: () => ({}),
    steps: false
  }),
  async mounted () {
    this.questions = await ky.get(`${baseUri}/questions`).json()

    this.steps = {
      current: 0,
      total: this.questions.length - 1
    }
  },
  methods: {
    answerChosen () {
      if (this.steps.current === this.steps.total) {
        return this.$emit('finished')
      }

      this.steps.current++
    }
  }
}
</script>
