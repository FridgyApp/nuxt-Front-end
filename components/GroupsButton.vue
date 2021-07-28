<template>
  <div class="text-center">
    <v-menu offset-y transition="fab-transition">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          class="navBar-Button mx-3 rounded-pill"
          color="#666"
          dark
          v-bind="attrs"
          elevation="2"
          v-on="on"
        >
          <v-icon>mdi-account-multiple</v-icon>
          Group
          <v-icon>mdi-menu-down</v-icon>
        </v-btn>
      </template>
      <v-list>
        <v-list-item v-for="(item, index) in items" :key="index">
          <v-list-item-title>{{ item.name }}</v-list-item-title>
        </v-list-item>
        <v-btn plain><FormAddUserGroup @addUserGroup="addUserGroup" /></v-btn>

        <!-- <v-list-item
          v-for="(item, index) in items"
          :key="index"
        >
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item> -->
      </v-list>
    </v-menu>
  </div>
</template>

<script>
export default {

  async fetch() {
    const {members, name} = await this.$axios.$get('/api/group')
    this.members = members
    this.$emit('updatename', name)
  },
  data: () => ({
    dialog: false,
    members: []
  }),
  computed: {
    items() {
      return this.members.concat({ name: 'Add User in Group' })
    }
  },
  methods: {
    addUserGroup(email) {

      this.$emit('addUserGroup',email)
      this.dialog = false
      this.email = ''
    },
  },
}
</script>