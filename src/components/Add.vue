<template>
  <div class="wrapper">
    <input type="text" placeholder="标题" class="title" v-model="title">
    <textarea class="doc" placeholder="链接内容" v-model="link" ></textarea>
    <button class="inverse btn" @click="clickHandle">生成二维码</button>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        link: '',
        title: ''
      }
    },
    methods: {
      clickHandle() {
        try{
          if(this.link.length === 0) {
            return alert('创建失败：链接不能为空');
          }
          if(this.title.length === 0) {
            this.title = this.link;
          }
          const param = {
            title: this.title,
            link: this.link,
            timestamp: Date.now(),
          };
          this.$emit('createQrcode', param)
        }catch (err) {
           alert('创建失败：' + err);
        }
      }
    }
  }
</script>
<style>
  .wrapper{
    width: 100%;
  }
  .btn {
    float: left;
  }
  .doc {
    width: 100%;
    height: 100px;
    resize: vertical;
  }
  .title {
    width: 100%;
  }
</style>
