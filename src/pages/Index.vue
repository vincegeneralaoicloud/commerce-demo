<template>
  <div align-items="right" class="col-9">
    <q-toolbar class="bg-grey-3">
      <div class="row">
        <q-select
          v-model="selectedProductType"
          :options="optionsProductTypes"
          behavior="menu"
          label="Filter"
          use-input

        >
          <template v-slot:no-option>
            <q-item>
              <q-item-section class="text-grey">
                Product not found
              </q-item-section>
            </q-item>
          </template>
        </q-select>
      </div>
    </q-toolbar>

    <div class="row">
      <div v-for="(product, index) in displayedProducts" :key="index">
        <div class="col-4">
          <q-badge v-if="product.isSale" color="red">Sale</q-badge>
          <q-card class="my-card" v-ripple>
            <img :src="product.productImage" fit="contain" />
            <q-card-section>
              <div class="text-h6">{{ product.productName }}</div>
              <div class="text-subtitle-2">{{ product.price }}</div>
            </q-card-section>
          </q-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "PageIndex",
  mounted() {
    this.loadProducts();
  },
  data() {
    return {
      products: [],
      optionsProductTypes: [],
      stringOptions: [],
      selectedProductType: "",
      displayedProducts: [],
    };
  },
  methods: {
    loadProducts() {
      this.products = [
        {
          index: 0,
          isSale: false,
          price: "$49.99",
          productImage: "Product_1.jpeg",
          productName: "Pure Blonde Crate",
          type: "Beer",
        },
        {
          index: 1,
          isSale: true,
          price: "$14.99",
          productImage: "Product_2.jpeg",
          productName: "Victoria Bitter 4x6x375ml",
          type: "Beer",
        },
        {
          index: 2,
          isSale: false,
          price: "$24.99",
          productImage: "Product_3.jpeg",
          productName: "Kirin Megumi 4x6x330ml",
          type: "Beer",
        },
        {
          index: 3,
          isSale: true,
          price: "$4.99",
          productImage: "Product_4.jpeg",
          productName: "Panhead CH Johnny Octane Can",
          type: "Beer",
        },
        {
          index: 4,
          isSale: false,
          price: "$25.99",
          productImage: "Product_5.jpeg",
          productName: "Aquila Spark SauvB Bottle",
          type: "Wine",
        },
        {
          index: 5,
          isSale: true,
          price: "$29.99",
          productImage: "Product_6.jpeg",
          productName: "Bernadino Spumante Bottle",
          type: "Wine",
        },
        {
          index: 6,
          isSale: true,
          price: "$69.99",
          productImage: "Product_7.jpeg",
          productName: "Grey Goose Orginal 10x12x50ml",
          type: "Spirits",
        },
        {
          index: 7,
          isSale: false,
          price: "$49.99",
          productImage: "Product_8.jpeg",
          productName: "Scrumpy RBerry 6x1.25L",
          type: "Cider",
        },
      ];

      const types = this.products.map((product) => product.type);

      this.stringOptions = [...new Set(types)];
      this.stringOptions.push("All");
      this.displayedProducts = this.products;
      this.optionsProductTypes = this.stringOptions;
    },
  },

  watch: {
    selectedProductType: function (val) {
      if (val !== "") {
        this.displayedProducts = this.products.filter(
          (product) => product.type === val
        );
      }

      if (val === "All") {
        this.displayedProducts = this.products;
      }
    },
  },
});
</script>
