<template>
  <div class="outter">
    <div class="wrapper">
      <VueQrcode :value="item.link" :options="{ width: 300, height: 300 }"></VueQrcode>
      <!--<div class="line"></div>-->
      <h3 class="title">{{item.title}}</h3>
      <div class="line"></div>
      <div class="bottom row">
        <div class="options col-md-6">
          <img src="../assets/edit.svg" alt="delete" class="icon"  @click="editClickHandle">
          <img src="../assets/trash-2.svg" alt="delete" class="icon" @click="deleteClickHandle">
        </div>
        <div class="col-md-6">
          <p class="desc">{{date}}</p>
          <p class="desc">{{time}}</p>
        </div>
      </div>
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
      this.date = `${d.toLocaleDateString()}`;
      this.time = d.toLocaleTimeString();
    },
    methods: {
      createQrcode() {

      },
      editClickHandle() {
        this.$emit('editItem', {timestamp: this.item.timestamp});
      },
      deleteClickHandle() {
        const confirmRes = confirm('Confirm delete?');
        if (confirmRes) {
          this.$emit('deleteItem', this.item)
        }
      }
    }
  }
</script>

<style scoped>
.outter {
  width: 100%;
  padding: 10px;
}
.line {
  height: 1px;
  width: 100%;
  background-color: #2c3e50;
  opacity: .3;
}
.wrapper{
  /*padding: 20px;*/
  padding:5px;
  border: solid rgba(51,51,51,.3) 1px;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  align-items: center;
  /*background-color: red;*/
}
.bottom {
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
}
.options {
  display: flex;
  flex-direction: row;
  justify-content: space-around;;
}
.icon {
  font-size: 20px;
}
.title {
  width: 95%;
  text-overflow: ellipsis;
  overflow: hidden;
}
.desc {
  font-size: 12px;
  line-height: 14px;
  color: #999;
  text-align: right;
}
.icon {
  width: 24px;
  height: 24px;
  cursor: pointer;
}
</style>