<template>
  <div class="cart-content clearfix">
    <div class="cart-goods clearfix" v-for="(cartItem, index) in cartList" :key="index">
      <div class="cart-list-item check-item">
        <input type="checkbox" name="" id="" v-model="cartItem.choosed">
      </div>
      <div class="cart-list-item">
        <div class="img-box" :style="{ backgroundImage: `url(${cartItem.img})` }"></div>
      </div>
      <div class="cart-list-item detail-item">
        <p class="cart-item-title">{{ cartItem.title }}</p>
        <p class="cart-item-price">¥ {{ cartItem.price | fixNumber }}</p>
        <div class="cart-item-control">
          <p class="cursor-pointer" @click="reduceHandler(index)">-</p>
          <p class="cart-item-num">{{ cartItem.num }}</p>
          <p class="cursor-pointer" @click="addHandler(index)">+</p>
        </div>
      </div>
    </div>
    <div>总价: {{ total }}</div>
  </div>
</template>

<script>
export default {
  name: 'cart',
  props: ['cartData'],
  data () {
    return {
      cartList: [],
    }
  },
  filters: {
    fixNumber(val) {
      return Number(Number(val).toFixed(2));
    }
  },
  computed: {
    total() {
      let count = 0;
      this.cartList.forEach((item) => {
        if (item.choosed) {
          count += item.num * item.price;
        }
      });
      return Number(count.toFixed(2));
    },
  },
  methods: {
    addHandler(index) {
      this.cartList[index].num += 1;
    },
    reduceHandler(index) {
      this.cartList[index].num = Math.max(this.cartList[index].num - 1, 0);
    },
  },
  watch: {
    cartData(newData) {
      let flag = false;
      for (let item of this.cartList) {
        if (item.code === newData.code) {
          item.num += 1;
          flag = true;
          break;
        }
      }
      if (!flag) {
        this.cartList.push({
          ...newData,
          num: 1,
          choosed: true,
        });
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.img-box {
  width: 100px;
  height: 100px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
}
.cart-goods {
  height: 100px;
  width: 100%;
}
.cart-list-item {
  float: left;
  height: 100%;
}
.check-item {
  width: 30px;
  padding: 40px 7px;
  box-sizing: border-box;
}
.detail-item {
  width: calc(100% - 130px);
  padding: 10px;
  box-sizing: border-box;
}
.detail-item p {
  margin: 0;
}
p.cart-item-title {
  margin-bottom: 5px;
}
.cart-item-price {
  float: left;
}
.cart-item-control {
  float: right;
  border-left: 1px solid #a1a1a1;
  border-top: 1px solid #a1a1a1;
}
.cart-item-control p {
  float: left;
  width: 20px;
  height: 20px;
  line-height: 20px;
  text-align: center;
  border-right: 1px solid #a1a1a1;
  border-bottom: 1px solid #a1a1a1;
}
p.cart-item-num {
  width: 50px;
}
.cursor-pointer {
  cursor: pointer;
}
</style>
