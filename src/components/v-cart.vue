<template>
  <div class="v-cart">
    <router-link :to="{name: 'catalog'}">
      <div class="v-catalog__link_to_cart">Back</div>
    </router-link>
    <h1>Cart</h1>
    <h4 v-if="!cart_data.length">There are no products in cart.</h4>
    <v-cart-item
      v-for="(item, index) in cart_data"
      :key="item.article"
      :cart_item_data="item"
      @deleteFromCart="deleteFromCart(index)"
      @increment ="increment(index)"
      @decrenebt = "decrement(index)"
    />
    <div class="v-cart__total">
      <p class="total__name">Total:</p>
      <p>{{cartTotalCost}} Lei</p>
    </div>
  </div>
</template>

<script>
import vCartItem from './v-cart-item'
import {mapActions} from 'vuex'

export default {
  name: "v-cart",
  components: {
    vCartItem
  },
  computed: {
    cartTotalCost() {
      let result = []

      if (this.cart_data.length) {
        for (let item of this.cart_data) {
          result.push(item.price * item.quantity)
        }
        result = result.reduce(function (sum, el) {
          return sum + el;
        })
        return result;
      }
      else {
        return 0;
      }
    }
  },
  methods: {
    ...mapActions([
      'DELETE_FROM_CART',
      'INCREMENT_CART_ITEM',
      "DECREMENT_CART_ITEM"
    ]),
    increment(index) {
      this.INCREMENT_CART_ITEM(index)
    },
    decrement(index) {
      this.DECREMENT_CART_ITEM(index)
    },
    deleteFromCart(index) {
      this.DELETE_FROM_CART(index)
    }
  },
  props: {
    cart_data: {
      type: Array,
      default() {
        return [];
      }
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/styles';

  .v-cart {
    margin-bottom: 100px;
    &__total {
      display: flex;
      justify-content: center;
      position: fixed;
      bottom: 0;
      right: 0;
      left: 0;
      padding: $padding*2 $padding*3;
      background-color: #2C3E50FF;
      font-family: Ubuntu,serif;
      font-size: 20px;
      color: #ffffff;
    }
    .total__name {
      margin-right: $margin*2;
    }
  }

</style>