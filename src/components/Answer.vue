<template>
  <button class="answer" @click="$emit('click')">
    <img :src="answer.image? answer.image : 'https://picsum.photos/800'" :alt="answer.name" class="answer-image" />

    <h2 class="answer-title">{{answer.name}}</h2>
    <span v-if="chosen" class="chosen">Dit is jouw gekozen antwoord.</span>
  </button>
</template>

<script>
export default {
  name: 'Answer',
  props: {
    answer: {
      type: Object,
      default: () => ({}),
      required: true
    },
    chosen: {
      type: Boolean,
      default: false
    }
  }
}
</script>
