<template>
  <v-card 
    class="mx-auto scroll"
    width="auto" 
    max-width="800" 
    max-height="550"
    min-width="400"
    min-height="625"
    overflow-visible>
    
    <v-toolbar color="#333" dark>
      <v-spacer></v-spacer>
        <v-toolbar-title><b>SHOPPING LIST</b></v-toolbar-title>
      <v-spacer></v-spacer>
      </v-toolbar>
    <v-list>
      <v-list-item v-for="(item, i) in listUpdate" :key="i">
        <v-list-item-content>
          <v-list-item-title v-text="item.productId.name"></v-list-item-title>
          <v-form>
            <v-text-field label="Comments" comments></v-text-field>

          </v-form>
        </v-list-item-content>
            <v-list-item-action>

            <v-btn
              class="mx-2"
              fab
              dark
              small
              color="primary"
              @click="deleteProduct(item)">
              <v-icon dark> mdi-trash-can-outline dark </v-icon>
            </v-btn>
            </v-list-item-action>
      </v-list-item>
    </v-list>
  </v-card>
</template>

<script>
export default {
  props: {
    list: Array,
  },
  computed: {
    listUpdate() {
      return this.list 
    }
  },
  methods: {
    deleteProduct(item) {
      
      this.$emit('erase',item.productId._id)
    },
  },
}
</script>

<style scoped>

.app_bar {
  max-height: calc(100vh - 64px);
  height: auto;

}
.scroll {
  overflow-y: scroll;
}

</style>

