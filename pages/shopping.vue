<template>
  <v-container fluid>
    <v-row no-gutters>
      <v-col cols="5" offset="-3" class="mt=3">
        <ProductsList @moveShopping="addList" :products = products /> 
        <v-container>
          <v-card class="d-flex flex-column justify-center">
            <v-text-field label="Products" outlined clearable></v-text-field>
            <v-btn x-large color="#1E88FF" dark depressed>
              Add New Product
            </v-btn>
          </v-card>
        </v-container>
        <v-spacer></v-spacer>
      </v-col>
      <v-col
        offset="2"
        class="d-flex flex-column justify-center"
        cols="4"
      >
      <ShoppingList @erase = deleteItem :list="list" />
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
export default {
  middleware: 'auth',
  async asyncData({ $axios }) {
    const [ products, list ]  = await Promise.all([$axios.$get('/api/products/60f045e39c653dce2e52837e'), $axios.$get('/api/shoppingList/')]) 

    return { products, list }
  },


  // async asyncData({ $axios }) {
  //   const [listProduct, shoppingList] = await Promise.all([
  //     $axios.$get('/api/products'),
  //     $axios.$get('/api/shoppingList'),
  //   ])
  //   return { apiProduct: listProduct, apiShopping: shoppingList }
  // },
  // data() {
  //   return {
  //     list: [],
  //     product: [],
  //     nameProduct: '',
  //   }
  // },
  // mounted() {
  //   this.product = this.apiProduct
  //   this.list = this.apiShopping
  // },
  methods: {
    async addList(product) {
      this.list= await this.$axios.$post('/api/shoppinglist', {
        productId: product,
      })
    },
    async deleteItem(id) {
      const hola= await this.$axios.$delete(`/api/shoppingList/${id}`)
      console.log(hola)
    },
  },
}
</script>

<style scoped>
.scroll {
  overflow: scroll;
}
.title {
  font-family: 'Raleway', sans-serif !important;
  color: #0066da;
}
::-webkit-scrollbar {
  display: none;
}
.app_bar {
  max-height: calc(100vh - 64px);
  height: auto;
  overflow-y: auto;
  overflow-x: hidden;
}
</style>