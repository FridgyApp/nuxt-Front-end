<template>
  <v-app>
    <div class="overflow-hidden">
      <v-app-bar color="grey darken-4" dark>
        <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>
        <v-toolbar-title>Home</v-toolbar-title>

        <v-spacer></v-spacer>
        <v-btn
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          exact
          elevation="2"
          color="transparent"
        >
          <v-icon>mdi-calendar-month</v-icon>
          {{ item.title }}
        </v-btn>
        <v-spacer></v-spacer>
        <v-btn elevation="2" color="transparent">
          <v-icon>mdi-cog-outline</v-icon>
          Settings
        </v-btn>
        <v-btn @click="logoutSesion" elevation="2" color="transparent">
          <v-icon>mdi-cog-outline</v-icon>
          Logout
        </v-btn>
      </v-app-bar>
      <v-navigation-drawer v-model="drawer" absolute temporary>
        <v-list nav dense>
          <v-list-item-group v-model="group" active-class="amber lighten-2">
            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-home</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Home</v-list-item-title>
            </v-list-item>

            <v-divider></v-divider>

            <v-list-item
              v-for="(item, i) in items"
              :key="i"
              :to="item.to"
              exact
              elevation="2"
              color="transparent"
            >
              <v-list-item-icon>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-item-icon>
              <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item>

            <v-divider></v-divider>

            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-cog-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Settings</v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-cog-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Logout</v-list-item-title>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-navigation-drawer>
    </div>
    <v-main>
      <Nuxt />
    </v-main>
    <v-footer class="footer" color="grey darken-4" padless app>
      <v-row justify="center" no-gutters>
        <v-col class="grey darken-4 py-4 text-center white--text" cols="12">
          {{ new Date().getFullYear() }} — <strong>Fridgy.App</strong>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  asyncData({$auth}){
    console.log($auth.user)
    return {nameGroup: $auth.nameGroup}
  },
  data() {
    return {
      drawer: false,
      group: null,
      items: [
        {
          icon: 'mdi-calendar-month',
          title: 'Home',
          to: '/',
        },
        {
          icon: 'mdi-cart-outline',
          title: 'Shopping List',
          to: '/shopping',
        },
        {
          icon: 'mdi-note-outline',
          title: 'Calendar',
          to: '/inspire',
        },
      ],
    }
  },
  methods: {
    async logoutSesion() {
      await this.$auth.logout()
    }
  },
}
</script>

<style scoped>
.white {
  color: #fff;
}
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>
