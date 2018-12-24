<template>
  <div class="wrapper">
    <input type="text" placeholder="title" class="title" v-model="title">
    <textarea class="doc" placeholder="qrcode content" v-model="link" ></textarea>
    <button class="inverse btn" @click="clickHandle">Create</button>
    <button class="primary btn" @click="updateClickHandle">Update</button>
    <button class=" btn" @click="cancelClickHandle">Cancel Update</button>
  </div>
</template>
<script>
  import Swal from 'sweetalert2';
  export default {
      props:['isUpdate', 'activeQrcode'],
      data() {
          return {
              link: '',
              title: ''
          }
      },
      created() {
          this.link = this.activeQrcode.link;
          this.title = this.activeQrcode.title;
      },
      methods: {
          clickHandle() {
              try{
                  if(this.link.length === 0) {
                      return Swal('Fail：Empty title or empty content');
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
                  Swal('Fail：' + err);
              }
          },
          cancelClickHandle() {
            this.$emit('cancel', {})
          },
          updateClickHandle() {
              if(this.link.length === 0) {
                  return Swal('Fail：Empty title or empty content');
              }
              if(this.title.length === 0) {
                  this.title = this.link;
              }
              const param = {
                  title: this.title,
                  link: this.link,
                  timestamp: Date.now(),
              };
            this.$emit('updateQrcode', {
              ...this.activeQrcode,
                ...param
            });
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
