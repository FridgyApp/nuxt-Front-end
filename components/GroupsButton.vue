<template>
  <div class="text-center">
    <v-menu offset-y transition="fab-transition">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          class="navBar-Button mx-3 rounded-pill"
          color="#666"
          dark
          v-bind="attrs"
          v-on="on"
          elevation="2"
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
        <v-btn plain><FormAddUserGroup /></v-btn>

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
    this.items = this.members
    this.items.push({ name: 'Add User in Group' })
  },
  methods: {
    addUserGroup() {
      this.$emit('addUserGroup', this.email)
      this.dialog = false
      this.email = ''
    },
  },
}
</script>