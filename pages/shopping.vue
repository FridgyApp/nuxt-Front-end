<template>
  <v-container>
    <v-row no-gutters>
      <v-col cols="5" class="mt-3">
        <v-container>
          <v-card>
          <ProductsList :products="products" @moveShopping="addList" />
          </v-card>
        </v-container>
        <v-container>
          <v-card
            class="d-flex flex-column justify-center"
            width="auto"
            max-width="600"
            overflow-visible
          >
            <v-text-field
            v-model="newProduct"
              label="Products"
              outlined
              clearable         
            ></v-text-field>
            <v-btn
              x-large
              color="#ffba01"
              light
              depressed
              @click="newProductList"
            >
              Add New Product
            </v-btn>
          </v-card>
        </v-container>
      </v-col>
      <v-col cols="5" class="mt-3">
        <v-container>
          <v-card>
          <ShoppingList :list="list" @erase="deleteItem" @editNoteProduct="editNoteProduct" />
          </v-card>
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
    async editNoteProduct({id, notes}){
       try {
        const hola = await this.$axios.$put(`/api/shoppingList/${id}`,{notes})
        console.log(hola)
      } catch (error) {}
    }
  },
}
</script>

<style scoped>
.title {
  font-family: 'Raleway', sans-serif !important;
  color: #ffba01;
}
</style>