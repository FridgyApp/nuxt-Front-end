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
          <v-col cols="3" class="stickyNote-bg">
            <StickyNote :notes="notes" ></StickyNote>
          </v-col>
          <v-col cols="6" class="calendar-bg"> </v-col>
          <v-col cols="3" class="shoppingList-bg">
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
  async asyncData({ $auth, $axios }) {
    const config = {
      headers: {
        token: $auth.$storage._state['_token.local'],
      },
    }
    const [list, notes ] = await Promise.all([ $axios.$get(`/api/shoppingList/`, { config }),  $axios.$get(`/api/stickynotes/`, { config })])
    return { list, notes }
  },
  methods: {
    async createGroup(name) {
      try {
        await this.$axios.$post(
          'api/group',
          { name, members: [] },
          {
            headers: {
              token: this.$auth.user,
            },
          }
        )
      } catch (error) {
        console.log('algo paso')
      }
    },
    async deleteItem(id) {
      this.list = await this.$axios.$delete(`/api/shoppingList/${id}`)
    },
    async addPostIt(note){
      const config = {
        headers: {
          token: this.$auth.$storage._state['_token.local'],
      },
    }
      const newNote = await  this.$axios.$post(`/api/stickynotes`, note, config)
      this.notes.push(newNote)
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
