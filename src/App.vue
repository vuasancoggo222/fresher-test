<script setup>
import Sidebar from "./components/Sidebar.vue";
import MainStore from "./components/MainStore.vue";
import { shopProducts } from "../data/storeProducts.json";
import { products } from "../data/product.json";
import { ref, reactive } from "vue";
const productList = ref([]);
const originalProductList = ref([]);
const sortType = ref('')
const filterOption = ref([])
let currentStore = reactive({
  name: "",
  id: "",
});
const getStoreSelected = (store) => {
  currentStore = {
    name: store.name,
    id: store.id,
  };
  const productsOfStore = shopProducts.filter(
    (product) => product.shop == store.id
  );
  let productsId = [];
  for (let i = 0; i < productsOfStore.length; i++) {
    productsId.push(productsOfStore[i].product);
  }
  const productsItem = products.filter((product) =>
    productsId.includes(product.id)
  );
  productList.value = productsItem;
  originalProductList.value = productsItem;
  sortProduct(sortType.value)
  if(filterOption.value.length){
    filterProduct(filterOption.value)
  }
};

const filterProduct = (filter) => {
  let products = [];
  filterOption.value = filter
  if (!filter.length) {
    getStoreSelected(currentStore);
  } else {
  
    for (let value of filter) {
      const filterProduct = originalProductList.value.filter((product) =>
        product.toppings.toLowerCase().includes(value.toLowerCase())
      );
      console.log(filterProduct);
      if (filterProduct.length) {
        products.push(...filterProduct);
      }
    }
    products = [...new Set(products)];

    productList.value = products;
    sortProduct(sortType.value)
  }
};

const sortProduct = (value) => {
  sortType.value = value
  const field = sortType.value.split('-')[0]
  const order = sortType.value.split('-')[1]
  if(order == 'asc'){
    productList.value = productList.value.sort((a, b) => (a[field] > b[field]) ? 1: -1);
  }
  else if(order == 'dsc'){
    productList.value = productList.value.sort((a, b) => (a[field] < b[field]) ? 1: -1);
  }
 
}
</script>

<template>
  <div class="wrapper">
    <div class="sidebar">
      <sidebar
        @store-selected="getStoreSelected"
        @default-store="defaultStore"
      />
    </div>
    <div class="main">
      <main-store
        @filter="filterProduct"
        @sort="sortProduct"
        :products-list="productList"
        :store-name="currentStore.name"
      />
    </div>
  </div>
</template>

<style>
.wrapper {
  min-height: 100vh;
  font-family: Roboto, sans-serif;

  display: grid;
  grid-template-columns: 20% 1fr;
}
@media screen and (max-width:768px) {
  .wrapper{
    grid-template-columns: 30% 1fr;
  }

}
@media screen and (max-width:360px) {
  .wrapper{
    grid-template-columns: none;
    grid-template-rows: 300px 1fr;
  }
}
.sidebar {
  background-color: #1d2c53;
}
.main {
  background-color: #f0f0f0;
}
</style>
