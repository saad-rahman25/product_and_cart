<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header class="top-bar spread">
    <nav class="top-bar-nav">
      <RouterLink to="/" class="top-bar-link">
        <i class="icofont-spoon-and-fork"></i>
        <span>Home</span>
      </RouterLink>
      <RouterLink to="/products" class="top-bar-link">
        <span>Products</span>
      </RouterLink>
      <RouterLink to="/past-orders" class="top-bar-link">
        <span>Past Orders</span>
      </RouterLink>
    </nav>
    <div @click="toggleSidebar" class="top-bar-cart-link">
      <i class="icofont-cart-alt icofont-1x"></i>
      <span>Cart ({{ totalQuantity }})</span>
    </div>
  </header>
      <!-- <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav> -->
    <!-- </div>
  </header> -->

    <RouterView :inventory="inventory" :addToCart="addToCart" />

    <Sidebar
      v-if="showSidebar" 
      :toggle="toggleSidebar" 
      :cart="cart"  
      :inventory="inventory"
      :remove="removeItem"
    />
</template>

<script>
  import Sidebar from './components/Sidebar.vue';
  // can also do '@/components/Sidebar.vue'; thus we can reference the src folder this way from anywhere
  import food from './food.json'

  export default {
    compenents: {
      Sidebar
    },
    data () {
      return {
        showSidebar: false,
        inventory: food,
        cart: {}
      }
      //  Thus all the variables in the parent component and we pass the variabels and functions to children;
      //  Children can call functions like addToCart that will then update the state of the parent component.
    },
    computed: {
      totalQuantity() {
        return Object.values(this.cart).reduce((acc, curr) => {    // for loop
          return acc+ curr
        }, 0)
      }
    },
    methods: {
      addToCart(name, index) {
        if (!this.cart[name]) this.cart[name] = 0
        this.cart[name] += this.inventory[index].quantity
        //console.log(this.cart)
        this.inventory[index].quantity = 0
      },
      toggleSidebar() {
        this.showSidebar = !this.showSidebar
      },
      removeItem(name) {
        delete this.cart[name]
      }
    },
  }

</script>


