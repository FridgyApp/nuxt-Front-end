<template>
  <v-app>
    <div class="overflow-hidden">
      <v-app-bar class="navBar" dark>
        <v-app-bar-nav-icon @click="drawer = true"></v-app-bar-nav-icon>
        <img src="../static/logo-blanco.png"
          width="180px">
        <v-spacer></v-spacer>
        <v-toolbar-title>La Casa de Pepe</v-toolbar-title>

        <v-spacer></v-spacer>
        <v-btn
          class="navBar-Button mx-3 rounded-pill"
 
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          exact
          elevation="2"
          color="#ffba01" light
        
        >
          <v-icon>{{ item.icon }}</v-icon>
          {{ item.title }}
        </v-btn>
        <v-spacer></v-spacer>
        <div>
          <GroupsButton />
        </div>    
        
        <v-btn 
          class="navBar-Button mx-3 rounded-pill"
          @click="logoutSesion" 
          elevation="2" 
          color="#666">
          <v-icon>mdi-logout</v-icon>
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

          </v-list-item-group>
        </v-list>
      </v-navigation-drawer>
    </div>
    <v-main class="bg-main">
      <Nuxt />
    </v-main>
    <v-footer class="footer" color="grey darken-4" padless app>
      <v-row justify="center" no-gutters>
        <v-col class="grey darken-4 py-4 text-center white--text" cols="12">
          {{ new Date().getFullYear() }} — <strong>Fridge.App</strong>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  asyncData({$auth}){
    console.log($auth.$storage)
    return {nameGroup: $auth.nameGroup}
  },
  data() {
    return {
      drawer: false,
      group: null,
      items: [
        {
          icon: 'mdi-home',
          title: 'Home',
          to: '/',
        },
        {
          icon: 'mdi-cart-outline',
          title: 'Shopping List',
          to: '/shopping',
        },
        {
          icon: 'mdi-calendar-month',
          title: 'Calendar',
          to: '/calendar',
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

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Dosis:wght@600&display=swap');
#app {
  font-family: 'Dosis', sans-serif;
}

.white {
  color: #fff;
}
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
}
</style>
