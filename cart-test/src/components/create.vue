<template>
  <div class="create-content">
    <div class="form-content clearfix">
      <div class="form-control img-control">
        <label for="create-file" class="img-uploader" :class="{ disabled: imgLoading }">
          <!-- <img v-if="imgData" :src="imgData" alt=""> -->
          <div class="img-box" :style="{ backgroundImage: `url(${imgData})` }"></div>
        </label>
        <input id="create-file" ref="imgFile" v-show="false" type="file" class="file" placeholder="ICCID" accept="image/*" capture="camera" @change="changeHandler">
      </div>
      <div class="form-control text-control">
        <input type="text" v-model="title" placeholder="名称">
        <input type="number" v-model="price" placeholder="价格">
      </div>
    </div>
    <div>
      <button @click="resetHandler">重置</button>
      <button @click="submitHandler">提交</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "create",
  data() {
    return {
      title: '',
      price: '',
      imgData: '',
      imgLoading: false,
      fileReader: new FileReader(),
      msg: "Welcome to Your Vue.js App"
    };
  },
  created() {
    this.fileReader.onload = (evt) => {
      this.imgData = evt.target.result;
      console.log(this.imgData);
      this.imgLoading = false;
    };
  },
  methods: {
    changeHandler(e) {
      if (this.imgLoading) {
        return;
      }
      console.log(e.target.files[0]);
      this.fileReader.readAsDataURL(e.target.files[0]);
      this.imgLoading = true;
    },
    resetHandler() {
      this.imgData = '';
      this.$refs.imgFile.value = '';
      this.title = '';
      this.price = '';
    },
    submitHandler() {
      if (!this.imgData || isNaN(this.price) || !this.title) {
        return;
      }
      this.$emit('add', {
        img: this.imgData,
        price: this.price,
        title: this.title,
        code: new Date().getTime(),
      });
      this.resetHandler();
    },
  }
};
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.create-content {
  padding: 10px;
  border-bottom: 1px solid #a1a1a1;
}
.form-content {
  padding-bottom: 10px;
}
.img-control {
  float: left;
}
.text-control {
  float: left;
  width: calc(100% - 102px);
  padding: 0 10px;
  box-sizing: border-box;
}
.text-control input {
  display: block;
  width: 100%;
  height: 30px;
  border: none;
  border-bottom: 1px solid #a1a1a1;
  outline: none;
}
.text-control input + input {
  margin-top: 29px;
}
.img-uploader {
  display: block;
  width: 100px;
  height: 100px;
  cursor: pointer;
  background-color: #a1a1a1;
  border: 1px solid #c1c1c1;
}
.img-uploader.disabled {
  pointer-events: none;
  cursor: default;
  opacity: 0.6; 
}
.img-box {
  width: 100px;
  height: 100px;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
}
</style>
