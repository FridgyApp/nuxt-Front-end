<template>
  <v-container fluid>
    <v-row>
      <v-col >
        <v-container class="justify-center" >
          <v-row>
            <v-col col="6"  class="justify-center mt-3">
              

                <v-card
                  class="d-flex flex-column justify-center"
                  width="auto"
                  max-width="600"
                  overflow-visible
                >
                <ProductsList :products="products" @moveShopping="addList" />
                  <v-text-field
                  v-model="newProduct"
                    label="Products"
                    outlined
                    clearable         
                  ></v-text-field>
                  <v-btn
                    large
                    color="#ffba01"
                    light
                    depressed
                    @click="newProductList"
                  >
                    Add New Product
                  </v-btn>
                </v-card>
              
            </v-col>  
          </v-row>    
        </v-container>
      </v-col>
      <v-col>
        <v-container class="justify-center">
          <v-row>
            <v-col cols="6" class="justify-center mt-4">
              <ShoppingList :list="list" @erase="deleteItem" />
            </v-col>
          </v-row>
        </v-container>
      </v-col>
    </v-row>
  </v-container>
</template>


<script>
export default {
  middleware: 'auth',
  async asyncData({ $axios, $auth }) {
    const [products, list] = await Promise.all([
      $axios.$get(`/api/products/${$auth.user.group}`),
      $axios.$get('/api/shoppingList/'),
    ])
    return { products, list }
  },
  data() {
    return {
      newProduct: '',
    }
  },
  methods: {
    async addList(product) {
      try {
        this.list = await this.$axios.$post('/api/shoppinglist', {
          productId: product,
        })
      } catch (error) {}
    },
    async deleteItem(id) {
      try {
        this.list = await this.$axios.$delete(`/api/shoppingList/${id}`)
      } catch (error) {}
    },
    async newProductList() {
      try {
        const newProduct = await this.$axios.$post(`/api/products/`, {
          name: this.newProduct,
        })
        this.products.push(newProduct)
      } catch (error) {
        console.log(error.response.data.message)
      }
    },
  },
}
</script>

<style scoped>
.title {
  font-family: 'Raleway', sans-serif !important;
  color: #ffba01;
}
</style>