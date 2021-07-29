<template>
  <div class="text-center">
    <v-menu offset-y transition="fab-transition">
      <template #activator="{ on, attrs }">
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
        <v-btn plain class="ml-2"> USERS IN GROUP </v-btn>
        <v-list-item  v-for="(item, index) in items" :key="index" class="ml-5">
          <v-list-item-title>{{ item.name }}</v-list-item-title>
        </v-list-item>
        <v-btn plain><FormAddUserGroup @addUserGroup="addUserGroup" /></v-btn>
      </v-list>
    </v-menu>
  </div>
</template>

<script>
export default {
  props: {
    members: Array,
  },
  data: () => ({
    dialog: false,
    email: '',
    items: [],
  }),
  mounted() {
    // this.items.members.push( { title: 'Add User in Group' })
    if (!this.members) this.items = []
    else {
      this.items = this.members
    }
  },
  methods: {
    addUserGroup(email) {
      this.$emit('addUserGroup', email)
      this.dialog = false
      this.email = ''
    },
  },
}
</script>