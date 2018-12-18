<template>
  <div class="outter">
    <div class="wrapper">
      <VueQrcode :value="item.link" :options="{ width: 300, height: 300 }"></VueQrcode>
      <div class="line"></div>
      <h3 class="title">{{item.title}}</h3>
      <p class="desc">{{date}}</p>
    </div>
  </div>
</template>

<script>
  import VueQrcode from '@chenfengyuan/vue-qrcode';
  export default {
    props:['item'],
    components: {VueQrcode},
    data() {return {date: this.item.timestamp}},
    created() {
      this.createQrcode();
      const d = new Date(this.item.timestamp);
      this.date = `${d.toLocaleDateString()} ${d.toLocaleTimeString()}`
    },
    methods: {
      createQrcode() {

      }
    }
  }
</script>

<style scoped>
.outter {
  width: 100%;
  padding: 20px;
}
.wrapper{
  padding: 20px;
  border: solid #333333 1px;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  align-items: center;
  /*background-color: red;*/
}
.title {
  width: 70%;
  text-overflow: ellipsis;
  overflow: hidden;
}
.desc {
  font-size: 16px;
  color: #999;
  text-align: left;
}
</style>