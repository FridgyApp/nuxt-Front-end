<template>
  <v-container fluid>
    <v-row class="px-4">
      <v-col cols="12" class="d-flex justify-end mb-2">
        <div class="d-flex justify-center align-center py-2">
          <FormAddGroup @addGroup="createGroup" />
          <FormAddPost-It @addPostIt="addPostIt" />
          <FormAddUserGroup @addUserGroup="addUserGroup" />          
        </div>
        <div>
          <GroupModal/>
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-container fluid>
        <v-row>
          <v-col cols="3" class="primary">
            <v-container>
              <v-row>
                <v-col v-for="note in notes" :key="note._id" cols="6">
                  <StickyNote
                    v-if="Array.isArray(notes)"
                    :note="note"
                    @delete="deleteNote"
                    @edit="editNote"
                  ></StickyNote>
                </v-col>
              </v-row>
            </v-container>
          </v-col>
          <v-col cols="6" class="calendar-bg">
            <Calendar :types="types" />
          </v-col>
          <v-col cols="3" class="shoppingList-bg">
            <ShoppingList
              v-if="Array.isArray(list)"
              :list="list"
              @erase="deleteItem"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-row>
  </v-container>
</template>

<script>
export default {
  middleware: 'auth',
  async asyncData({ $axios }) {
    const [list, notes, events] = await Promise.all([
      $axios.$get(`/api/shoppingList/`),
      $axios.$get(`/api/stickynotes/`),
      $axios.$get(`/api/events`),
    ])
    const types = events.map((event) => {
      return {
        ...event,
        start: new Date(event.start).getTime(),
        end: new Date(event.end).getTime(),
      }
    })
    return { list, notes, types }
  },
  methods: {
    async createGroup(name) {
      try {
        await this.$axios.$post('api/group', { name, members: [] })
      } catch (error) {
        console.log('Can not create group')
      }
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
  },
}
</script>

<style scoped>
.stickyNote-bg {
  display: grid;
  background-color: white;
  width: 100%;
  min-height: 700px;
}

.calendar-bg {
  display: grid;
  background-color: #ffba01;
}

.shoppingList-bg {
  display: grid;
  background-color: white;
}
</style>
