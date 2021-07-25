<template>
  <v-container fluid>
    <v-row no-gutters>
      <v-card
        class="mx-auto scroll"
        width="auto"
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
            v-for="(item,key) in listProduct"
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
    </v-row>
  </v-container>

</template>

<script>
export default {
  props: {
    products: Array,
  },

  data: () => ({
    items: {
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
      'Fish': {
        action: 'mdi-fish',
        active: true,
        items: {},
      },
      'Meat': {
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
      'Drinks': {
        action: 'mdi-beer',
        active: true,
        items: {},
      },
      'Home': {
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
      'Own Products': {
        action: 'mdi-account',
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
      this.products.forEach(product =>{
        if(!this.items[product.category].items[product._id]){
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