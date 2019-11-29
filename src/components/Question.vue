<template>
  <section class="question">
    <Answer :answer="{ name: 'Complex' }" @click="alertWindow" />
  </section>
</template>

<script>
import Answer from '@/components/Answer'
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
  methods: {
    alertWindow () {
      window.alert('test')
    }
  }
}
</script>
