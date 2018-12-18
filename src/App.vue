<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
        <Header />
        <Add @createQrcode="createQrcodeHandle" />
      </div>
      <div class="col-md-12">
        <QrcodeList :list="qrcodeList"/>
      </div>
    </div>
  </div>
</template>

<script>
import Add from './components/Add.vue';
import Header from './components/Header.vue';
import QrcodeList from './components/QrcodeList.vue';
export default {
  name: 'app',
  data(){
    return {
      qrcodeList:[]
    }
  },
  components: {
    Add,
    Header,
    QrcodeList
  },
  methods: {
    createQrcodeHandle(param){
      if(!param || !param.title || !param.link) {
        return alert('创建失败: 不能为空');
      }
      if(this.filterQrcodeList(param)) {
        return alert('该内容已经存在');
      }
      this.qrcodeList = [param].concat(this.qrcodeList);
    },
    filterQrcodeList(param) {
     const len = this.qrcodeList.length;
     for (let i = 0; i < len; i++) {
       const item = this.qrcodeList[i];
       if(item.title === param.title && item.link === param.link) {
           return true;
       }
     }
      return false;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
