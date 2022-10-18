<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
      <router-view :event="event" />
      <router-link :to="{ name: 'EventDetails' }">Details</router-link>
      |
      <router-link :to="{ name: 'EventRegister' }">Register</router-link>
      |
      <router-link :to="{ name: 'EventEdit' }">Edit</router-link>
  </div>
</template>

<script>// @ts-nocheck
import EventService from '@/services/EventService.js'
export default {
  props: ['id'],
  data() {
    return {
      event: null
    }
  },
  created() {
    EventService.getEvent(this.id)
      .then(response => {
        this.event = response.data
      })
      .catch(error => {
        if (error.response && error.response.status == 404) {
          this.$router.push({
          name: '404Resource',
          params: { resource: 'event' }
        })
        } else {
          this.$router.push({ name: 'NetworkError' })
        }

        console.log(error)


      })
  }
}
</script>
