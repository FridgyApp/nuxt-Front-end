<template>
  <div>
    <v-alert
      color="cyan"
      border="left"
      elevation="2"
      colored-border
      icon="mdi-bulletin-board"
      ><v-row align="center">
        <v-col class="grow"> First of all, why don't you start by creating a group? </v-col>
        <v-col class="shrink">
          <FormAddGroup @addGroup="createGroup" />
        </v-col>
      </v-row>
    </v-alert>
  </div>
</template>

<script>
export default {
  data() {
    return {
      error: ''
    }
  },
  methods: {
    async createGroup(name) {
      try {
        const user = await this.$axios.$post('api/group', { name, members: [] })
        await this.$auth.setUser(user)
        this.$nuxt.refresh()
        location. reload()
      } catch (error) {
        this.error = error
      }
    },
  },
}
</script>