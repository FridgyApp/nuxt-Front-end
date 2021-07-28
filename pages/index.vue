<template>
  <v-container fluid fill-height class=" bgcolor prueba">
    <v-row>
      <v-col>
        <v-container v-if="Array.isArray(list)" fluid>
          <v-row>
            <v-col
              xl="12"
              md="6"
              lg="3"
              class="shoppingList-bg order-xs-2 order-sm-1 order-lg-0"
            >
              <ShoppingList :list="list" @erase="deleteItem" />
            </v-col>
            <v-col
              xl="12"
              md="12"
              lg="6"
              class="calendar-bg order-xs-1 order-sm-3 order-lg-1"
            >
              <Calendar
                @deleteEvent="deleteEvent"
                @editEvent="editEvent"
                :types="types"
              />
            </v-col>
            <v-col
              xl="12"
              md="6"
              lg="3"
              class="stickyNote-bg order-xs-0 order-sm-0 order-lg-2"
            >
              <v-container>
                <v-row>
                  <v-col>
                    <FormAddPost-It @addPostIt="addPostIt" />
                  </v-col>
                  <v-col v-for="note in notes" :key="note._id">
                    <StickyNote
                      :note="note"
                      @delete="deleteNote"
                      @edit="editNote"
                    ></StickyNote>
                  </v-col>
                </v-row>
                <v-row> </v-row>
              </v-container>
            </v-col>
          </v-row>
        </v-container>
        <v-container v-else d-flex justify-space-around>
          <GroupModal />
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  middleware: 'auth',
  data() {
    return {
      list: [],
    }
  },
  async asyncData({ $axios }) {
    try {
      const {
        name,
        shoppingList: list,
        stickyNotes: notes,
        events,
      } = await $axios.$get('/api/group')
      if (name) {
        const types = events.map((event) => {
          return {
            ...event,
            start: new Date(event.start).getTime(),
            end: new Date(event.end).getTime(),
          }
        })
        return {
          list,
          notes,
          types,
        }
      }
      return {
        list,
        notes,
        types: events,
      }
    } catch (error) {}
  },
  methods: {
    async createGroup(name) {
      try {
        const user = await this.$axios.$post('api/group', { name, members: [] })
        await this.$auth.setUser(user)
        this.$nuxt.refresh()
      } catch (error) {
        console.log('Can not create group')
      }
    },
    async editNoteProduct({ id, notes }) {
      try {
        await this.$axios.$put(`/api/shoppingList/${id}`, { notes })
      } catch (error) {}
    },

    async deleteItem(id) {
      this.list = await this.$axios.$delete(`/api/shoppingList/${id}`)
    },
    async deleteNote(id) {
      this.notes = await this.$axios.$delete(`/api/stickynotes/${id}`, {})
    },
    async editNote(note) {
      const modifiedNote = await this.$axios.$put(
        `/api/stickynotes/${note._id}`,
        { note }
      )
      this.notes = this.notes.map((note) => {
        if (note._id === modifiedNote._id) return modifiedNote
        else return note
      })
    },
    async addPostIt(note) {
      const newNote = await this.$axios.$post(`/api/stickynotes`, note)
      this.notes.push(newNote)
    },
    async addUserGroup(email) {
      try {
        const user = await this.$axios.$put(`/api/group`, { email })
        console.log(user)
      } catch (error) {
        console.log({ error })
      }
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
.stickyNote-bg {
  background-color: #666;
  
}
.bgcolor{
  background-color: #FFD96F
}
.calendar-bg {
  background-color: #666;
  
}

.shoppingList-bg {
  background-color: #666;
  
}
.prueba{
  height: 100%;
}
</style>
