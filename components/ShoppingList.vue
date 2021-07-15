<template>
  <v-card width="500" class="mx-auto" max-width="500" max-height="600">
    <v-toolbar color="#1E88FF" dark>
      <v-spacer></v-spacer>
      <v-toolbar-title><b>SHOPPING LIST</b></v-toolbar-title>

      <v-spacer></v-spacer>
    </v-toolbar>
    <v-list>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title ></v-list-item-title>
          <v-form>
            <v-text-field label="Solo" solo></v-text-field>
            <v-btn
              class="mx-2"
              fab
              dark
              small
              color="primary"
            >
              <v-icon dark> mdi-minus </v-icon>
            </v-btn>
          </v-form>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script>
export default {
  props: {
    list: Array,
  },
  // computed: {
  //   shoppingList() {
  //     return this.list[0].products.filter((buy) => !buy.bought)
  //   },
  // },
  // methods: {
  //   async updateList(item) {
  //     const listUpdate = await this.$axios.$put('/api/shoppingList/changeStatus', { id: item._id })
  //     this.$emit('isbuy',listUpdate)
  //   },
  // },
}
</script>