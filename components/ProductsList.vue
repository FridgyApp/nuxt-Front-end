<template>
  <v-container fluid>
    <v-row no-gutters>
      <v-card
        class="mx-auto scroll"
        width=auto
        max-height="600"
        overflow-visible
      >
        <v-app-bar color="#333" dark class="app_bar">
          <v-spacer></v-spacer>
          <v-toolbar-title><b>PRODUCTS</b></v-toolbar-title>
          <v-spacer></v-spacer>
        </v-app-bar>

        <v-list>
          <v-list-group
            v-for="item in listProduct"
            :key="item.uid"
            v-model="item.active"
            :prepend-icon="item.action"
            no-action
          >
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title
                  v-text="item.title"
                  class="title"
                ></v-list-item-title>
              </v-list-item-content>
            </template>

            <v-list-item
              v-for="child in item.items"
              :key="child.name"
              @click="addProduct(child)"
            >
              <v-list-item-content>
                <v-list-item-title v-text="child.name"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
        </v-list>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    products: Array,
  },

  data: () => ({
    items: [
      {
        action: 'mdi-fruit-pineapple',
        active: true,
        items: [],
        title: 'Fruits & Vegetables',
      },
      {
        action: 'mdi-baguette',
        active: true,
        items: [],
        title: 'Bread & Cakes',
      },
      {
        action: 'mdi-cheese',
        active: true,
        items: [],
        title: 'Milk & Cheese',
      },
      {
        action: 'mdi-fish',
        active: true,
        items: [],
        title: 'Fish',
      },
      {
        action: 'mdi-food-steak',
        active: true,
        items: [],
        title: 'Meat',
      },
      {
        action: 'mdi-shaker-outline',
        active: true,
        items: [],
        title: 'Ingredients & Spices',
      },
      {
        action: 'mdi-ice-cream',
        active: true,
        items: [],
        title: 'Frozen & Pre-Cooked Food',
      },
      {
        action: 'mdi-pasta',
        active: true,
        items: [],
        title: 'Cereals & Pasta',
      },
      {
        action: 'mdi-cookie',
        active: true,
        items: [],
        title: 'Snacks & Sweets',
      },
      {
        action: 'mdi-beer',
        active: true,
        items: [],
        title: 'Drinks',
      },
      {
        action: 'mdi-spray-bottle',
        active: true,
        items: [],
        title: 'Home',
      },
      {
        action: 'mdi-bottle-tonic-plus',
        active: true,
        items: [],
        title: 'Cleanliness & Health',
      },
      {
        action: 'mdi-paw',
        active: true,
        items: [],
        title: 'Pet Products',
      },
      {
        action: 'mdi-shovel',
        active: true,
        items: [],
        title: 'Garden & Home',
      },
      {
        action: 'mdi-account',
        active: true,
        items: [],
        title: 'Own Products',
      },
    ],
  }),
  computed: {
    listProduct() {
      this.addProducts()
      return this.items
    },
  },
  methods: {
    addProducts() {
      this.products.forEach((product) => {
        this.items.forEach((categ) => {
          if (categ.title === product.category) {
            categ.items = [...categ.items, product]
          }
        })
      })
    },
    addProduct(product) {
      this.$emit('moveShopping', product._id)
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
  color: #333;
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