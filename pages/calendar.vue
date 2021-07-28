<template>
  <v-container class="calendar-wr" fill-height>
    <v-row>
      <v-col cols="12" class="d-flex justify-end">
        <FormEvent @addEvent="addEvent" />
      </v-col>

      <v-col cols="12" class="calendar-bg">
        <Calendar
          @deleteEvent="deleteEvent"
          @editEvent="editEvent"
          :types="types"
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
  methods: {
    async addEvent(eventCalendar) {
      try {
        const event = await this.$axios.$post('/api/events', eventCalendar)
        event.start = new Date(event.start).getTime()
        event.end = new Date(event.end).getTime()
        this.types.push(event)
      } catch (error) {}
    },
    async deleteEvent(id) {
      try {
        await this.$axios.$delete(`/api/events/${id}`)

        this.types = this.types.filter((event) => event._id !== id)
        console.log(this.types)
      } catch (error) {
        console.log(error)
      }
    },
    async editEvent({ id, ...event }) {
      try {
        const eventUpdate = await this.$axios.$put(`/api/events/${id}`, event)
        console.log(eventUpdate, event)
        eventUpdate.start = new Date(event.start).getTime()
        eventUpdate.end = new Date(event.end).getTime()
        this.types = await this.types.map((ev) => {
          if (ev._id === eventUpdate._id) {
            console.log('entre')
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
  background-color: #ffba01;
}
.calendar-wr {
  background-color: #ffba01;
}
</style>
