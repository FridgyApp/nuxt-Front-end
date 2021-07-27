<template>
  <div class="text-center">
    <v-menu offset-y>
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
        <v-list-item
          v-for="(item, index) in items"
          :key="index"
        >
          <v-list-item-title>{{ item.title }}</v-list-item-title>
        </v-list-item>
      </v-list>
    </v-menu>
  </div>
</template>

<script>
  export default {
    data: () => ({
      items: [
        { title: 'Members' },
        { title: 'Add User to your Group' },
       
      ],
    }),
  }
</script>