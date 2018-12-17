<template>
  <div class="wrapper">
    <fieldset>
      <legend>在线多 QRCODE 生成服务</legend>
      <textarea class="doc" placeholder="链接内容" v-model="link" />
      <button class="primary btn" ><span class="icon-edit "></span> 生成二维码</button>
    </fieldset>
  </div>
</template>
<script>
  import Qrcode from 'qrcode';
  export default {
    data() {
      return {
        link: ''
      }
    },
    methods: {
      createQrcode() {

      }
    }
  }
</script>
<style>
  .btn {
    float: left;
  }
  .doc {
    width: 100%;
    height: 100px;
    resize: vertical;
  }
</style>
