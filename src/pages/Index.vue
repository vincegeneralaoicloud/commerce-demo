<template>
  <div class="container">
    <div class="col-12">
      <q-toolbar class="bg-grey-3">
        <div class="row fit justify-end">
          <q-select
            v-model="selectedProductType"
            :options="optionsProductTypes"
            behavior="menu"
            label="Filter"
            use-input
            clearable
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
    </div>

    <div class="q-pa-md row items-start q-gutter-md">
      <div v-for="(product, index) in displayedProducts" :key="index">
        <q-card class="my-card" v-ripple>
          <q-img src="product.productImage" :fit="'contain'">
            <template v-slot:error>
              <div
                class="absolute-full flex flex-center bg-negative text-white"
              >
                Cannot load image
              </div>
            </template>
          </q-img>
          <q-card-section>
            <div class="text-h7 h7">{{ product.productName }}</div>
            <div class="text-subtitle-2">{{ product.price }}</div>
          </q-card-section>
          <q-badge v-if="product.isSale" color="red" floating>Sale</q-badge>
        </q-card>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import axios from "axios";

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
    async loadProducts() {
      try {
        const localApiServer = "http://localhost:3000";

        const apiResponse = await axios.get(localApiServer + "/products");

        this.products = apiResponse.data;
      } catch (apiResponseError) {
        console.log("apiResponseError ", JSON.stringify(apiResponseError));
      }

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

      if (val === "All" || val === null) {
        this.displayedProducts = this.products;
      }
    },
  },
});
</script>



<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
</style>
