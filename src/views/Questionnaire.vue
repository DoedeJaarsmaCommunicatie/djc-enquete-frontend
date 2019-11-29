<template>
  <main id="main">
    <Question :question="{}" />
  </main>
</template>

<script>
import Answer from '@/components/Answer'
import { baseUri } from '@/config'
import ky from 'ky'
import Question from '@/components/Question'

export default {
  name: 'Questionnaire',
  components: { Question, Answer },
  data: () => ({
    questions: () => ({})
  }),
  async mounted () {
    this.questions = await ky.get(`${baseUri}/questions/`)

    console.dir(this.questions)
  }
}
</script>
