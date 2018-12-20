<template>
  <div class="wrapper">
    <input type="text" placeholder="title" class="title" v-model="title">
    <textarea class="doc" placeholder="qrcode content" v-model="link" ></textarea>
    <div class="action-wrapper">
      <button class="inverse btn" @click="clickHandle">Create</button>
      <button class="primary btn load-btn" @click="$emit('loadQrcodeList')">Load From Storage</button>
    </div>
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
            return alert('Fail：Empty title or empty content');
          }
          if(this.title.length === 0) {
            this.title = this.link;
          }
          const param = {
            title: this.title,
            link: this.link,
            timestamp: Date.now(),
          };
          this.$emit('createQrcode', param);
          this.link = '';
          return this.title = '';
        }catch (err) {
           alert('Fail：' + err);
        }
      }
    }
  }
</script>
<style>
  .wrapper{
    width: 100%;
  }
  .action-wrapper {
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
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
  .load-btn {
    float: right;
  }
</style>
