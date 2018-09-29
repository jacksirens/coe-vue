<template>
  <div class="list-content">
    <div class="clearfix">
      <div class="list-item" v-for="(listItem, index) in listData" :key="index">
        <div class="list-item-img" :style="{ backgroundImage: `url(${listItem.img})` }"></div>
        <p>{{ listItem.title }}</p>
        <p>¥ {{ listItem.price | fixNumber }}</p>
        <button @click="cartHandler(index)">加入购物车</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'list',
  props: ['list'],
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  computed: {
    listData() {
      return this.list;
    }
  },
  filters: {
    fixNumber(val) {
      return Number(Number(val).toFixed(2));
    }
  },
  methods: {
    cartHandler(index) {
      const item = this.listData[index];
      this.$emit('addCart', item);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.list-content {
  height: calc(100% - 156px);
  overflow-y: auto;
}
.list-item {
  float: left;
  margin: 5px;
  width: 108px;
  padding: 4px;
  border: 1px solid #a1a1a1;
}
.list-item-img {
  width: 100px;
  height: 100px;
  background-color: #f1f1f1;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
}
.list-item p {
  margin: 0;
}
</style>
