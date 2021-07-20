<template>
  <v-container fluid>
    <v-row no-gutters>
      <v-col cols="5" offset="-3" class="mt=3">
        <ProductsList :products = products @moveShopping="addList"  /> 
        <v-container>
          <v-card class="d-flex flex-column justify-center">
            <v-text-field label="Products" outlined clearable></v-text-field>
            <v-btn x-large color="success" dark depressed>
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
      <ShoppingList :list="list" @erase = deleteItem />
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
export default {
  middleware: 'auth',
  async asyncData({ $axios }) {
    const user = localStorage.getItem('userLogin')
    const [ products, list ]  = await Promise.all([$axios.$get(`/api/products/${user.group}`), $axios.$get('/api/shoppingList/')]) 
    console.log(list)
    return { products, list }
  },
  methods: {
    async addList(product) {
      this.list= await this.$axios.$post('/api/shoppinglist', {
        productId: product,
      })
      console.log(this.list)
    },
    async deleteItem(id) {
      this.list = await this.$axios.$delete(`/api/shoppingList/${id}`)
    },
    newProductList(){

    }
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