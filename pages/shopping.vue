<template>
  <v-container >
    <v-row no-gutters>
      <v-col 
        cols="5"
        class="mt-3">
        <v-container>
         <ProductsList :products = products @moveShopping="addList"  /> 
        </v-container>
        <v-container>
          <v-card
           class="d-flex flex-column justify-center" 
           width="auto"
           max-width="600"
           overflow-visible
           >
            <v-text-field label="Products" outlined clearable></v-text-field>
            <v-btn x-large color="#ffba01" light depressed>
              Add New Product
            </v-btn>
          </v-card>
        </v-container>    
      </v-col>
      <v-col
        cols="5"
        class="mt-3">
      <v-container>
      <ShoppingList :list="list" @erase = deleteItem />
      </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
export default {
  middleware: 'auth',
  async asyncData({ $axios, $auth }) {
    const [ products, list ]  = await Promise.all([$axios.$get(`/api/products/${$auth.user.group}`), $axios.$get('/api/shoppingList/')]) 
    return { products, list }
  },
  methods: {
    async addList(product) {
      this.list= await this.$axios.$post('/api/shoppinglist', {
        productId: product,
      })
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
  color: #FFBA01;
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