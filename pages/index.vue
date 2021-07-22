<template>
  <v-container fluid>
    <v-row class="px-4">
      <v-col cols="12" class="d-flex justify-end mb-2">
        <div class="d-flex justify-center align-center py-2">
          <FormAddGroup @addGroup="createGroup" />
          <FormAddPost-It @addPostIt="addPostIt" />
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
                    :note="note"
                    @delete="deleteNote"
                    @edit="editNote"
                  ></StickyNote>
                </v-col>
              </v-row>
            </v-container>
          </v-col>
          <v-col cols="6" class="success"> </v-col>
          <v-col cols="3" class="accent">
            <ShoppingList @erase="deleteItem" :list="list" />
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
    const [list, notes] = await Promise.all([
      $axios.$get(`/api/shoppingList/`),
      $axios.$get(`/api/stickynotes/`),
    ])
    return { list, notes }
  },
  methods: {
    async createGroup(name) {
      try {
        await this.$axios.$post(
          'api/group',
          { name, members: [] }
        )
      } catch (error) {
        console.log('algo paso')
      }
    },
    async deleteItem(id) {
      this.list = await this.$axios.$delete(`/api/shoppingList/${id}`)
    },
    async deleteNote(id) {
      this.notes = await this.$axios.$delete(
        `/api/stickynotes/${id}`,
        {}
      )
    },
    async editNote(note) {
      this.notes = await this.$axios.$put(
        `/api/stickynotes/${note._id}`,
        {note}
      )
    },
    async addPostIt(note) {
      const newNote = await this.$axios.$post(`/api/stickynotes`, note)
      this.notes.push(newNote)
    },
  },
}
</script>
