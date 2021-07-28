<template>
  <v-container class="shopping-bg">
    <v-row>
      <!-- left container -->
      <v-col cols="6" class="d-flex flex-column justify-center mt-3">
        <v-container>
          <v-row>
            <v-col class="pa-0">
              <ProductsList :products="products" @moveShopping="addList" />
            </v-col>
            <v-col>
              <v-container>
                <v-row class="pt-5">
                  <v-spacer></v-spacer>
                  <v-col cols="7" class="d-flex align-center">
                    <v-text-field
                      dense
                      v-model="newProduct"
                      label="Products"
                      outlined
                      clearable
                      hide-details
                    ></v-text-field>
                  </v-col>
                  <v-col class="d-flex align-center">
                    <v-btn
                      class="align-self-end"
                      large
                      color="#ffba01"
                      light
                      depressed
                      @click="newProductList"
                    >
                      Add New Product
                    </v-btn>
                  </v-col>
                  <v-spacer></v-spacer>
                </v-row>
              </v-container>
            </v-col>
          </v-row>
        </v-container>
      </v-col>
      <!-- right container -->
      <v-col cols="6" class="justify-center mt-3">
        <ShoppingList
          :list="list"
          @erase="deleteItem"
          @editNoteProduct="editNoteProduct"
        />
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
    async editNoteProduct({ id, notes }) {
      try {
        const hola = await this.$axios.$put(`/api/shoppingList/${id}`, {
          notes,
        })
        console.log(hola)
      } catch (error) {}
    },
  },
}
</script>

<style scoped>
.title {
  font-family: 'Raleway', sans-serif !important;
  color: #ffba01;
}

.shopping-bg{
  background-color: #ffba01;
}
</style>