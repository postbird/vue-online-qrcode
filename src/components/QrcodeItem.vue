<template>
  <div class="outter">
    <div class="wrapper">
      <div class="qrcode-wrapper">
        <div @click="qrcodeClickHandle">
          <VueQrcode :value="item.link" :options="{ width: 300, height: 300 }" ></VueQrcode>
        </div>
        <div class="qrcode-mask" v-show="showMask" @click="qrcodeMaskClickHandle"></div>
      </div>
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
  import Swal from 'sweetalert2';
  import EventEmitter from 'EventEmitter';
  const EM = new EventEmitter();
  import VueQrcode from '@chenfengyuan/vue-qrcode';
  export default {
    props:['item'],
    components: {VueQrcode},
    data() {return {date: '', time: '', showMask: false}},
    created() {
      this.createQrcode();
      const d = new Date(this.item.timestamp);
      this.date = `${d.toLocaleDateString()}`;
      this.time = d.toLocaleTimeString();
      this.initQrcodeMaskEvent();
    },
    methods: {
      createQrcode() {

      },
      editClickHandle() {
        this.$emit('editItem', {timestamp: this.item.timestamp});
      },
      deleteClickHandle() {
          Swal({
            title: 'Confirm delete?',
            type: 'question',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes'
          }).then((result) => {
              if (result.value) {
                this.$emit('deleteItem', this.item)
              }
          })

      },
      initQrcodeMaskEvent() {
        // emitter qrcode mask event
        EM.on('SHOW_QROCDE_MASK', (timestamp) => {
          this.showMaskEMHandled = true;
          if(timestamp === this.item.timestamp) return false;
          this.showMask = true;
        });
        EM.on('HIDE_QROCDE_MASK', (timestamp) => {
            this.showMask = false;
            this.showMaskEMHandled = false;
        });
      },
      qrcodeClickHandle() {
          if (this.showMaskEMHandled) {
            // if has been showed mask, need to hide all masks
            this.showMaskEMHandled = false;
            return EM.emit('HIDE_QROCDE_MASK', this.item.timestamp);
          }
          this.showMaskEMHandled = true; // showed mask flag
          EM.emit('SHOW_QROCDE_MASK', this.item.timestamp);
      },
      qrcodeMaskClickHandle() {
          EM.emit('HIDE_QROCDE_MASK', this.item.timestamp);
          EM.emit('SHOW_QROCDE_MASK', this.item.timestamp);
      },
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
.qrcode-wrapper {
  position: relative;
  cursor: pointer;
}
.qrcode-mask {
  position: absolute;
  top:0;
  left:0;
  right:0;
  bottom: 0;
  background-color: #212121;
  opacity: 0.97;
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
  font-size: 20px;
  font-weight: 600;
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