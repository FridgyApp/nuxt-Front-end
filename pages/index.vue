<template>
  <v-container fluid>
    <v-row class="px-4">
      <v-col cols="12" class="d-flex justify-end mb-2">
        <div class="d-flex justify-center align-center py-2">
          <FormAddGroup @addGroup="createGroup" />
          <FormAddPost-It />
        </div>
      </v-col>
    </v-row>
    <v-row>
      <v-container fluid>
        <v-row>
          <v-col cols="3" class="primary"></v-col>
          <v-col cols="6" class="success"> </v-col>
          <v-col cols="3" class="accent"><ShoppingList /></v-col>
        </v-row>
      </v-container>
    </v-row>
  </v-container>
</template>
<script>
export default {
  middleware: 'auth',
  methods: {
    async createGroup(name) {
      try {
        await this.$axios.$post(
          'api/group',
          { name, members: [] },
          {
            headers: {
              token: this.$auth.$storage.setUniversal('userId'),
            },
          }
        )
      } catch (error) {
        console.log('algo paso')
      }
      
    },
  },
}
</script>
