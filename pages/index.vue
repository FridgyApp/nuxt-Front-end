<template>
  <v-row>
    <v-col class="text-center">
      <img src="/v.png" alt="Vuetify.js" class="mb-5" />
      <blockquote class="blockquote">
        &#8220;First, solve the problem. Then, write the code.&#8221;
        <footer>
          <small>
            <em>&mdash;John Johnson</em>
          </small>
        </footer>
      </blockquote>
    </v-col>
    <v-btn depressed color="primary" @click="createGroup"> Primary </v-btn>
    <Modals />
  </v-row>
</template>
<script>
export default {
  middleware: 'auth',
  methods: {
    async createGroup() {
      const group = await this.$axios.$post('/api/group', {
        name: 'Adrian Prueba',
      })
      console.log(group)
    },
  },
}
</script>
