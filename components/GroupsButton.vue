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
       <v-btn plain>Group Members</v-btn><br>
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
    data: () => ({
      dialog: false,
      email:''
    }),
    methods: {
        addUserGroup() {
            this.$emit('addUserGroup', this.email)
            this.dialog = false
            this.email = ''
        }
    },

  }
</script>