<template>
  <v-container fluid fill-height class=" bgcolor">
    <v-row>
      <v-col>
        <v-container v-if="Array.isArray(list)" fluid>
          <v-row>
            <v-col
              xl="3"
              sm="12"
              md="6"
              lg="3"
        
              class="shoppingList-bg order-2 order-sm-2 order-lg-0"
            >
              <ShoppingList :list="list" @erase="deleteItem" @editNoteProduct="editNoteProduct" />
            </v-col>
            <v-col
              xl="6"
              sm="12"
              md="12"
              lg="6"
              class="calendar-bg order-1 order-sm-3 order-lg-1"
            >
              <Calendar
                :types="types"
                @deleteEvent="deleteEvent"
                @editEvent="editEvent"
              />
            </v-col>
            <v-col
              xl="3"
              sm="12"
              md="6"
              lg="3"
              class="stickyNote-bg order-0 order-lg-2"
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
          <GroupModal @refreshGroup="refreshGroup" />
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  middleware: 'auth',
   async asyncData({ $axios }) {
    try {
      const {
        name,
        members,
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
          name,
          members,
        }
      }
      return {
        list,
        notes,
        types: events,
        name,
        members,
      }
    } catch (error) {}
  },
    data() {
    return {
      error: ''
    }
  },
  mounted() {
    this.$nuxt.$emit('infoGroup',{ name:this.name, members:this.members})
  },

  methods: {
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
        await this.$axios.$put(`/api/group`, { email })
      } catch (error) {
        
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
      } catch (error) {
        this.error = error
      }
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
</style>
