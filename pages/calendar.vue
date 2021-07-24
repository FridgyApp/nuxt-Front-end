<template>
  <v-container>
    <v-row>
      <v-col cols="10">
        <Calendar @deleteEvent="deleteEvent" :types="types" />
      </v-col>
      <v-col cols="2">
        <v-container fill-height>
          <v-row>
            <v-col cols="12">
              <FormEvent @addEvent="addEvent" />
            </v-col>
          </v-row>
        </v-container>
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
    async deleteEvent(id){
      try {
        await this.$axios.$delete(`/api/events/${id}`)
        this.types = this.types.filter(event=>event._id !== id)
      } catch (error) {
        
      }
    }
  },
}
</script>
