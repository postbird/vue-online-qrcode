<template>
  <div class="wrapper">
    <input type="text" placeholder="title" class="title" v-model="title">
    <textarea class="doc" placeholder="qrcode content" v-model="link" ></textarea>
    <div class="action-wrapper">
      <button class="inverse btn" @click="clickHandle">Create</button>
      <div class="right-wrapper">
        <Datepicker v-model="date" format="yyyy/MM/dd" :full-month-name="true"></Datepicker>
        <button class="primary btn load-btn" @click="loadStorageHandle">Load From Storage</button>
      </div>
    </div>
  </div>
</template>
<script>
    import Swal from 'sweetalert2';
    import Datepicker from 'vuejs-datepicker';
    import {QRCODE_STORE_PRE_KEY} from '../config/constant';
    const date = new Date();
    export default {
    components:{Datepicker},
    data() {
      return {
        link: '',
        title: '',
        date: date.toLocaleDateString(),
      }
    },
    computed: {

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
          this.$emit('createQrcode', param);
          this.link = '';
          return this.title = '';
        }catch (err) {
           Swal('Fail：' + err);
        }
      },
      loadStorageHandle() {
          console.log(this.date);
        this.$emit('loadQrcodeList', this.date);
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
  .right-wrapper {
    display: flex;
    justify-content: center;;
    align-items: center;;
  }
</style>
