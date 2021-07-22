<template>
  <v-container>
    <v-row>
      <v-col cols="10">
        <Calendar :types="types" />
      </v-col>
      <v-col cols="2" >
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

  async asyncData({$axios}) {
    const events = await $axios.get('/api/events')
    const types = events.data.map((event) => {
        return {
          ...event,
          start: new Date(event.start).getTime(),
          end: new Date(event.end).getTime(),
        }
      })
    return { 
      types
    }
  },
  methods:{
    async addEvent(){
      const hola = {
        name:"hola",
        color:"red",
        description:"Con Bruno",
        start: new Date('2021-07-22'),
        end: new Date('2021-07-23'),
        timed: true
      }
      const event = await this.$axios.$post('/api/events', hola)
      event.start = new Date(event.start).getTime()
      event.end = new Date(event.end).getTime()
      this.types.push(event)
    }
  }
}
</script>
