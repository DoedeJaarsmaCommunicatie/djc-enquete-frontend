<template>
  <main id="main">
    <Answer :answer="{ name: 'Complex' }" />
  </main>
</template>

<script>
import Answer from '@/components/Answer'
export default {
  name: 'Questionnaire',
  components: { Answer }
}
</script>
