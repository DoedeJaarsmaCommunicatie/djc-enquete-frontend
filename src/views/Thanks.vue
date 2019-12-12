<template>
  <form class="form container mx-auto" @submit.prevent="store">
    <h2>Bedankt voor het invullen</h2>
    <p>
      Als laatste hebben wij nog wat vragen, als deze correct zijn kan je op verstuur klikken en gaan wij aan de slag.
      Is er iets niet correct? Dan kan je jouw gegevens nog aanpassen voor het versturen.
    </p>

    <div class="inputs">
      <div class="form-group" v-for="field in fields" :key="field.name">
        <label :for="field.name" class="sr-only">{{ field.label }}</label>
        <input :name="field.name"
               :class="field.class"
               :id="field.name"
               :type="field.type"
               v-model="field.value"
               :placeholder="field.label"
               required
        />
      </div>
    </div>
    <input type="submit" value="Opslaan" class="submit-button">

    <h2 v-if="finished">
      Bedankt voor het invullen van de vragenlijst. hij is in goede orde ontvangen.
    </h2>
    <h2 v-if="error">
      Er is een probleem, zijn alle velden goed ingevuld?
    </h2>
  </form>
</template>

<script>
import ky from 'ky'
import { baseUri } from '@/config'
export default {
  name: 'Thanks',
  data: () => ({
    fields: [
      { name: 'firstName', type: 'text', class: 'control', value: '', label: 'Voornaam' },
      { name: 'lastName', type: 'text', class: 'control', value: '', label: 'Achternaam' },
      { name: 'email', type: 'email', class: 'control email', value: '', label: 'E-mail' },
      { name: 'address', type: 'text', class: 'control', value: '', label: 'Adres' },
      { name: 'postalCode', type: 'text', class: 'control', value: '', label: 'Postcode' },
      { name: 'city', type: 'text', class: 'control', value: '', label: 'Plaats' }
    ],
    finished: false,
    error: false
  }),
  mounted () {
    const user = this.$store.state.user
    if (user) {
      this.fields[0].value = user.firstName
      this.fields[1].value = user.lastName
      this.fields[2].value = user.email
      this.fields[3].value = user.address
      this.fields[4].value = user.postalCode
      this.fields[5].value = user.city
    }
  },
  methods: {
    async store () {
      const user = this.$store.state.user
      if (!user) {
        return
      }

      const data = {
        firstName: this.fields[0].value,
        lastName: this.fields[1].value,
        email: this.fields[2].value,
        address: this.fields[3].value,
        postalCode: this.fields[4].value,
        city: this.fields[5].value
      }

      const userReq = await ky.post(`${baseUri}/user/${user.uuid}`, { json: data })
      const questions = await ky.post(`${baseUri}/finished/${user.id}`)

      if (userReq.status === 200 && questions.status === 204) {
        this.finished = true
      } else {
        this.error = true
      }
    }
  }
}
</script>

<style scoped lang="scss">
  h2 { font-size: 1.7rem; margin-bottom: 1.5rem; }
  p { margin-bottom: 1rem; }
</style>
