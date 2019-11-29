<template>
  <article class="answer">
    <img :src="answer.image? answer.image : 'https://picsum.photos/800'" :alt="answer.name" class="answer-image" />

    <h2 class="answer-title">{{answer.name}}</h2>
  </article>
</template>

<script>
export default {
  name: 'Answer',
  props: {
    answer: {
      type: Object,
      default: () => ({}),
      required: true
    }
  }
}
</script>
