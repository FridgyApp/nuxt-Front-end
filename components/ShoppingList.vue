<template>
  <v-card
    class="mx-auto"
    width="auto"
    max-width="800"
    max-height="250"
    min-width="200"
    min-height="525"
    overflow-visible
  >
    <v-toolbar color="#333" dark>
      <v-spacer></v-spacer>
      <v-toolbar-title><b>SHOPPING LIST</b></v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-list class="scroll" min-height="580" max-height="580">
      <v-list-item v-for="(item, i) in listUpdate" :key="i">
        <v-list-item-content>
          <v-list-item-title v-text="item.productId.name"></v-list-item-title>
          <v-form>
            <v-text-field :id="item._id" :value="item.notes" label="Comments"  ></v-text-field>
          </v-form>
        </v-list-item-content>
            <v-list-item-action>
            <v-btn
              class="mx-2"
              fab
              dark
              small
              color="primary"
              @click="editProduct(item._id)">
              <v-icon dark> mdi-check </v-icon>
            </v-btn><br>
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
  data() {
    return {
      comments: ''
    }
  },
  computed: {
    listUpdate() {
      return this.list
    },
  },
  methods: {
    deleteProduct(item) {
      this.$emit('erase', item.productId._id)
    },
    editProduct(id){
      const product = document.getElementById(`${id}`).value
       this.$emit('editNoteProduct',{id, notes:product})
    }
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

