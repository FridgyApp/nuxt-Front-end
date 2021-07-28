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
        <v-btn plain class="ml-2" >
          USERS IN GROUP
        </v-btn>
        <v-list-item  class="ml-5" v-for="(item, index) in items" :key="index">
          <v-list-item-title>{{ item.name }}</v-list-item-title>
        </v-list-item>
        <v-btn plain><FormAddUserGroup /></v-btn>
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
    
    this.items = this.members
    
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