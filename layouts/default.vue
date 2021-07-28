<template>
  <v-app>
    <div class="overflow-hidden">
      <v-app-bar class="navBar" dark app>
        <v-container fluid>
          <v-row>
            <v-col cols="4" class="d-flex align-center">
              <v-app-bar-nav-icon
                @click="drawer = true"
                class="d-md-none"
              ></v-app-bar-nav-icon>
              <img src="../static/logo-blanco.png" width="180px" />
              <v-toolbar-title class="ml-8">{{ name }}</v-toolbar-title>
            </v-col>
            <v-col cols="4" class="d-flex align-center">
              <v-btn
                v-for="(item, i) in items"
                :key="i"
                class="navBar-Button mx-3 rounded-pill d-sm-none d-md-flex"
                :to="item.to"
                exact
                elevation="2"
                color="#ffba01"
                light
              >
                <v-icon>{{ item.icon }}</v-icon>
                {{ item.title }}
              </v-btn>
            </v-col>
            <v-col cols="4" class="d-flex justify-end align-center">
              <GroupsButton
                @updatename="updatename"
                @addUserGroup="addUserGroup"
              />
              <v-btn
                class="navBar-Button mx-3 rounded-pill"
                elevation="2"
                color="#666"
                @click="logoutSesion"
              >
                <v-icon>mdi-logout</v-icon>
                Logout
              </v-btn>
            </v-col>
          </v-row>
        </v-container>
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
    <v-footer class="footer" color="grey darken-4" padless>
      <v-row justify="center" no-gutters>
        <v-col class="grey darken-4 py-4 text-center white--text" cols="12">
          {{ new Date().getFullYear() }} — <strong>Fridg.App</strong> — by Bruno
          Aggierni
          <a href="https://www.linkedin.com/in/bruno-aggierni/"
            ><v-icon color="blue">mdi-linkedin</v-icon></a
          >, Álvaro Poncio
          <a href="https://www.linkedin.com/in/alvaro-poncio/"
            ><v-icon color="blue">mdi-linkedin</v-icon></a
          >, Adrian Duran
          <a href="https://www.linkedin.com/in/adrian-duran-gomez/"
            ><v-icon color="blue">mdi-linkedin</v-icon></a
          >
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'defaultLayout',
  data() {
    return {
      name: '',
      members: [],
      nameGroup: '',
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
    },
    async addUserGroup(email) {
      try {
        const user = await this.$axios.$put(`/api/group`, { email })
        this.members.unshift(user)
      } catch (error) {
        console.log({ error })
      }
    },
    updatename(name) {
      this.name = name
    },
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Dosis:wght@600&display=swap');
#app {
  font-family: 'Dosis', sans-serif;
}
.bg-main {
  background-color: #ffd96f;
}
.white {
  color: #fff;
}
.footer {
  bottom: 0;
  width: 100%;
}
</style>
