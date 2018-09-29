<template>
  <div class="body clearfix">
    <div class="list-body pull-left">
      <create @add="addHandler"></create>
      <list :list="listData" @addCart="cartHandler"></list>
    </div>
    <div class="cart-body">
      <cart :cartData="cartData"></cart>
    </div>
  </div>
</template>

<script>
import create from './create'
import list from './list'
import cart from './cart'

export default {
  name: 'container',
  components: {
    create,
    list,
    cart
  },
  data () {
    return {
      listData: [],
      cartData: {}
    }
  },
  methods: {
    addHandler(data) {
      console.log(data);
      this.listData.push(data);
    },
    cartHandler(data) {
      this.cartData = {
        ...data,
        time: new Date().getTime(),
      };
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.body {
  position: relative;
}
.list-body {
  float: left;
  width: calc(100% - 299px);
  height: 100%;
  border-right: 1px solid #a1a1a1;
}
.cart-body {
  position: absolute;
  right: 0;
  top: 0;
  width: 300px;
  height: 100%;
  overflow-y: auto;
}
</style>
