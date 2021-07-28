<template>
      <v-card
        class="mx-auto"
        width="700"
        max-height="550"
        overflow-visible
      >
        <v-app-bar color="#333" dark class="app_bar">
          <v-spacer></v-spacer>
          <v-toolbar-title><b>PRODUCTS</b></v-toolbar-title>
          <v-spacer></v-spacer>
        </v-app-bar>

        <v-list class="scroll" max-height="500">
          <v-list-group
            v-for="(item, key) in listProduct"
            :key="key"
            v-model="item.active"
            :prepend-icon="item.action"
            no-action
          >
            <template #activator>
              <v-list-item-content>
                <v-list-item-title
                  class="title"
                  v-text="key"
                ></v-list-item-title>
              </v-list-item-content>
            </template>

            <v-list-item
              v-for="child in item.items"
              :key="child._id"
              @click="addProduct(child)"
            >
              <v-list-item-content>
                <v-list-item-title v-text="child.name"></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
        </v-list>
      </v-card>
</template>

<script>
export default {
  props: {
    products: Array,
  },

  data: () => ({
    items: {
      'Own Products': {
        action: 'mdi-account',
        active: true,
        items: {},
      },
      'Fruits & Vegetables': {
        action: 'mdi-fruit-pineapple',
        active: true,
        items: {},
      },
      'Bread & Cakes': {
        action: 'mdi-baguette',
        active: true,
        items: {},
      },
      'Milk & Cheese': {
        action: 'mdi-cheese',
        active: true,
        items: {},
      },
      Fish: {
        action: 'mdi-fish',
        active: true,
        items: {},
      },
      Meat: {
        action: 'mdi-food-steak',
        active: true,
        items: {},
      },
      'Ingredients & Spices': {
        action: 'mdi-shaker-outline',
        active: true,
        items: {},
      },
      'Frozen & Pre-Cooked Food': {
        action: 'mdi-ice-cream',
        active: true,
        items: {},
      },
      'Cereals & Pasta': {
        action: 'mdi-pasta',
        active: true,
        items: {},
      },
      'Snacks & Sweets': {
        action: 'mdi-cookie',
        active: true,
        items: {},
      },
      Drinks: {
        action: 'mdi-beer',
        active: true,
        items: {},
      },
      Home: {
        action: 'mdi-spray-bottle',
        active: true,
        items: {},
      },
      'Cleanliness & Health': {
        action: 'mdi-bottle-tonic-plus',
        active: true,
        items: {},
      },
      'Pet Products': {
        action: 'mdi-paw',
        active: true,
        items: {},
      },
      'Garden & Home': {
        action: 'mdi-shovel',
        active: true,
        items: {},
      },
    },
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
        if (!this.items[product.category].items[product._id]) {
          this.items[product.category].items[product._id] = product
        }
      })
    },
    addProduct(product) {
      this.$emit('moveShopping', product._id)
    },
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Dosis:wght@600&display=swap');

.scroll {
  overflow-y: scroll;
}
.title {
  font-family: 'Dosis', sans-serif !important;
  color: #333;
}

.app_bar {
  max-height: calc(100vh - 64px);
  height: auto;
}
</style>
