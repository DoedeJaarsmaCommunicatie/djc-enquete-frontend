<template>
  <section class="question">
    <header class="question-header">
      {{ question.name }}
    </header>
    <section class="answers" v-if="quest">
      <Answer
        v-for="answer in quest.options"
        :key="answer.id"
        :answer="answer"
        @click="answerChosen(answer)"
        :chosen="answer.id === chosen"
      />
    </section>
  </section>
</template>

<script>
import Answer from '@/components/Answer'
import { baseUri } from '@/config'
import ky from 'ky'

export default {
  name: 'Question',
  components: { Answer },
  props: {
    question: {
      type: Object,
      default: () => ({}),
      required: true
    }
  },
  data: () => ({
    quest: () => ({}),
    chosen: false
  }),
  methods: {
    async fetchQuestion () {
      this.quest = await ky.get(`${baseUri}/questions/${this.question.id}`).json()
    },
    async answerChosen (answer) {
      if (this.$store.state.user) {
        const id = this.$store.state.user.uuid
        answer = await ky.post(`${baseUri}/answer/${id}/${this.question.id}`, { json: { answer: answer.id } })
        this.$emit('picked', { answer, question: this.quest })
      }
    },
    async choiceFetch () {
      if (this.$store.state) {
        const id = this.$store.state.user.uuid
        const chosen = await ky.get(`${baseUri}/answer/${id}/${this.question.id}`).json()
        this.chosen = chosen.option_id
      }
    },
    fixHeight () {
      if (screen.width > 748) return

      const questions = document.querySelectorAll('.question')
      const header = document.querySelector('.header')

      for (let i = 0; i < questions.length; i++) {
        questions[i].style.height = `calc(100% - ${header.clientHeight})`
      }
    }
  },
  mounted () {
    this.fixHeight()
    this.fetchQuestion()
    this.choiceFetch()
  }
}
</script>

<style lang="scss">
  .question {
    /*height: calc(100% - 9rem);*/
    height: 100%;
    margin-top: 112px;

    @media screen and (max-width: 768px) {
      overflow: scroll;
    }
  }

  .question-header {
    padding: 1.5rem;
    text-align: center;
    font-size: 1.5rem;
    font-weight: bold
  }

  .answers {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  @media screen and (min-width: 769px) {
    .answers {
      align-items: center;
      justify-content: center;
      flex-direction: row;
    }
    .answer:not(:last-of-type) {
      margin-right: 1.5rem;
    }

    .question {
      display: flex;
      flex-direction: column;
      justify-content: center;
      margin-top: 0;
    }

    .question-header {
      font-size: 1.875rem;
      padding-top: 2rem;
    }
  }
</style>
