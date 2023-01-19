<script setup> 
import { onMounted, ref, watch } from 'vue';
import {products} from '../../data/product.json'
const props = defineProps(["productsList","storeName"])
const emit = defineEmits(['filter'])
const isFilter = ref(false)
const filterOption = ref([])
const sortOption = ref('name-asc')
onMounted(() => {
    emit("sort",sortOption.value) 
})
const toogleFilter = () => isFilter.value = !isFilter.value
const listToppings = ref([])
for(let i = 0 ; i < products.length; i ++){
    let toppings = products[i].toppings.split(",")
    for(let topping of toppings){
        topping = topping.toLowerCase().trim()
        topping = topping.charAt(0).toUpperCase() + topping.slice(1);
        listToppings.value = [...listToppings.value,topping]
    }   
}
listToppings.value = new Set(listToppings.value)
watch(filterOption,(value) =>{
    emit('filter',value)
})
watch(sortOption,(value)=>{
    emit("sort",value)
})
</script>

<template>
  <div class="main-store-wrapper">
    <h1 class="store-name">{{ storeName }} Menu</h1>
    <div class="action">
        <button class="filter-button" :class="{'active-filter' : isFilter}" @click="toogleFilter">Filter</button>
        <div class="sort">
          <span class="sort-text">Sort By</span>
          <select class="select-sort-option" name="" id="" v-model="sortOption">
            <option value="name-asc">Name Asc</option>
            <option value="name-dsc">Name Dsc</option>
            <option value="price-asc">Price Asc</option>
            <option value="price-dsc">Price Dsc</option>
        </select>
        </div>
    </div>
    <div  v-if="isFilter"  class="filter">
        <span class="toppings-text">Toppings :</span>
        <div class="toppings-wrapper">
            <div class="toppings" v-for="topping in listToppings">
            <input class="checkbox" type="checkbox" :value="topping" v-model="filterOption">
            <span>{{ topping }}</span>
        </div>
        </div>
        </div>
    <div class="product-list">
            <div class="product-item" v-for="product in props.productsList" key="product.id">
                  <span class="product-id">{{ product.id }}</span>
                  <span class="product-name">{{ product.name }}</span>
                  <span class="line"></span>
                  <div class="toppings-list">
                    <span class="toppings-text">Toppings :</span>
                    <span class="toppings-name">{{ product.toppings }}</span>
                  </div>
                  <div class="price-and-trending">
                    <span class="trending" v-if="product.trending">Trending</span>
                    <span v-else></span>
                    <span class="price">{{ new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(product.price) }}</span>
                   </div>
            </div>
    </div>
    <div class="no-product" v-if="!props.productsList.length">No products exist.</div>
  </div>

</template>

<style>
.main-store-wrapper{
    padding: 20px 50px;
}
.store-name{
    text-align: center;
    color : #1d2c53;
    margin-bottom: 30px;
}
.action{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.filter-button{
    padding: 12px 50px;
    background-color: #1d2c53;
    color: #ccc;
    border: none;
    font-size: 20px;
    border-radius: 4px;
    cursor: pointer;
}
.sort-text{
    font-size: 20px;
    margin-right: 20px;
    font-weight: 550;
}
select{
appearance: none;
padding: 12px 80px 12px 20px;
border:2px solid #1d2c53;
border-radius: 6px;
font-size: 16px;
background-color: #e4e4e4;
}
.product-list{
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    column-gap: 40px;
    margin-top: 30px;
}
.product-item{
   margin-bottom: 30px;
    background-color: #ffffff;
    padding: 30px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    border-radius: 6px;
  
}
.product-item  span{
    display: block;
}
.product-id{
    font-size: 24px;
    margin-bottom: 15px;
    font-weight: 500;
}
.product-name{
    font-size: 24px;
    color: #1d2c53;
    font-weight: bold;
    margin-bottom: 15px;
}
.line{
    height: 3px;
    background-color: #1d2c53;
    margin-bottom: 40px;
}
.toppings-list{
    margin-bottom: 50px;
}
.toppings-text{
    font-size: 20px;
    font-weight: 600;
    color: #1d2c53;
    margin-bottom: 10px;
}

.toppings-name{
    font-size: 20px;
    font-weight: 400;
    color: #1d2c53;
}
.price-and-trending{
    display: flex ;
    height: 50px;
    justify-content: space-between;
    align-items: center;
}
.trending{
    margin-left: -30px;
    display: inline-block;
    padding: 6px 20px;
    
    background-color: #1d2c53;
    color: #ffffff;
    font-size: 20px;
    font-weight: 550;
}
.price{
    display: inline-block;
    color: #1d2c53;
    font-size: 220%;
    font-weight: 550;
}
.filter{
    margin-top: 30px;
    padding: 30px;
    background-color: #ffffff;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.toppings-wrapper{
    margin-top: 20px;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    margin-right: 50px;
    gap: 15px;
    font-size: 20px;
    font-weight: 500;
    color: #1d2c53;
}
.checkbox{
    margin-right: 10px;
}

.active-filter{
    font-weight: 550;
    color: #ffffff;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}
.toppings-list{
    height: 80px;
}
.no-product{
    display: block;
    text-align: center;
    font-size: 20px;
    font-weight: 600;
}
@media screen and (max-width:768px) {
    .product-list{
    display: grid;
    grid-template-columns: 47% 47%;
    column-gap: 20px;
    margin-top: 20px;
}
.filter-button{
    padding: 10px 30px;
    background-color: #1d2c53;
   
}
select{
padding: 12px 0px 12px 0px;
}
.action{
    justify-content: space-around;
}
.filter{
    padding: 10px;
}
.toppings-wrapper{
    gap:10px;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
}
.price {
    margin-left: 5px;
    font-size: 120%;
}
.toppings-list{
    height: 80px;
}
}
@media screen and (max-width:360px) {
    .main-store-wrapper {
    padding: 4px;
}
.sort{
    margin-top: 10px;
}
    .product-list{
    display: grid;
    grid-template-columns: 1fr;
   
    margin-top: 20px;
}
.toppings-wrapper{
    margin-right: 0;
}
.filter-button{
    padding: 10px 10px;
    background-color: #1d2c53;
   
}
select{
padding: 10px 0px 10px 0px;
}
.action{
    display: block;
    justify-content: none;
}
.filter{
    padding: 10px;
   
}
.toppings-wrapper{
    gap:10px;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  
}
.price {
    margin-left: 5px;
    font-size: 120%;
}
.toppings-list{
    height: 40px;
}

}
</style>