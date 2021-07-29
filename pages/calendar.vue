<template>
  <v-container class="calendar-wr">
    <v-row>
      <v-col cols="12" class="d-flex justify-end mt-3">
        <FormEvent @addEvent="addEvent" />
      </v-col>

      <v-col cols="12" class="calendar-bg">
        <Calendar
          :types="types"
          @deleteEvent="deleteEvent"
          @editEvent="editEvent"
        />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    const events = await $axios.get('/api/events')
    const types = events.data.map((event) => {
      return {
        ...event,
        start: new Date(event.start).getTime(),
        end: new Date(event.end).getTime(),
      }
    })
    return {
      types,
    }
  },
    data() {
    return {
      error: ''
    }
  },
  methods: {
    async addEvent(eventCalendar) {
      try {
        const event = await this.$axios.$post('/api/events', eventCalendar)
        event.start = new Date(event.start).getTime()
        event.end = new Date(event.end).getTime()
        this.types.push(event)
      } catch (error) {
        this.error = error
      }
    },
    async deleteEvent(id) {
      try {
        await this.$axios.$delete(`/api/events/${id}`)

        this.types = this.types.filter((event) => event._id !== id)
      } catch (error) {
         this.error = error
      }
    },
    async editEvent({ id, ...event }) {
      try {
        const eventUpdate = await this.$axios.$put(`/api/events/${id}`, event)
        eventUpdate.start = new Date(event.start).getTime()
        eventUpdate.end = new Date(event.end).getTime()
        this.types = await this.types.map((ev) => {
          if (ev._id === eventUpdate._id) {
            return eventUpdate
          }
          return ev
        })
      } catch (error) {}
    },
  },
}
</script>

<style scoped>
.calendar-bg {
  background-color: #FFD96F;
}
.calendar-wr {
  background-color: #FFD96F;
}
</style>
