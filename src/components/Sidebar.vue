<script setup>

import { onMounted,ref } from 'vue';
import {stores} from '../../data/stores.json';
const emit = defineEmits(['defaultStore'])
const activeStore = ref(stores[0].id)
onMounted(() => {
    const defaultStore = stores[0]
    selectStore(defaultStore)
})
const selectStore = (store) => {
    emit('storeSelected',store)
    activeStore.value = store.id
}
</script>

<template>
  <div class="sidebar-wrapper">
   
    <h1 class="stores-name">Milk Tea Store</h1>
    <nav class="store-navigation">
    <ul>
    <li v-for="store in stores" :key="store.id" @click="selectStore(store)" :class="{'active' : store.id == activeStore }" class="store">{{ store.name }}</li>
    
    </ul>
    </nav>
  </div>

</template>
<style>
.sidebar-wrapper{
    width: 100%;
    padding-top: 15px;
}
.stores-name{
    text-align: center;
    color: #fff;
    font-weight: 600;
    margin-bottom: 40px;
}
ul{
 padding: 0;
}

.store{
    display: block;
    margin:15px 0;
    text-align: center;
    padding: 20px 0;
    font-size: 22px;
    color: #616b87;
    cursor: pointer;
}
.active{
    color : #fff;
    background-color: #2e4476;

}
@media screen and (max-width:360px) {
    .sidebar-wrapper{
    width: 100%;
    padding-top: 15px;
}
.stores-name{
    text-align: center;
    color: #fff;
    font-weight: 600;
    margin-bottom: 10px;
}
ul{
 padding: 0;
}

.store{
    display: block;
    margin:15px 0;
    text-align: center;
    padding: 10px 0;
    font-size: 18px;
    color: #616b87;
    cursor: pointer;
} 
}
</style>