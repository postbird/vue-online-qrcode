<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
        <Header ></Header>
        <Add @createQrcode="createQrcodeHandle" v-if="!activeQrcode"></Add>
        <Update
            v-if="activeQrcode"
            @createQrcode="createQrcodeHandle"
            @updateQrcode="updateQrcodeHandle"
            @cancel="cancelUpdateQrcodeHandle"
            :activeQrcode="activeQrcode"
            :isUpdate="update"
        ></Update>
      </div>
      <div class="col-md-12">
        <QrcodeList :list="qrcodeList" @editItem="editItemHandle" @deleteItem="deleteItemHandle"></QrcodeList>
      </div>
    </div>
  </div>
</template>

<script>
import Add from './components/Add.vue';
import Update from './components/Update.vue';
import Header from './components/Header.vue';
import QrcodeList from './components/QrcodeList.vue';
export default {
  name: 'app',
  data(){
    return {
      qrcodeList: [],
      activeQrcode:null,
    }
  },
  components: {
    Add,
    Header,
    QrcodeList,
    Update
  },
  computed: {
    update() {
      return !!this.activeQrcode;
    }
  },
  methods: {
    createQrcodeHandle(param){
      if(!param || !param.title || !param.link) {
          return alert('Fail：Empty title or empty content');
      }
      const filterTime = this.filterQrcodeList(param);
      if(filterTime) {
        return alert(`Fail: Existed! Create Time: ${this.getDateTimeFromTimestamp(filterTime)} `);
      }
      // reset
      this.activeQrcode = null;
      this.qrcodeList = [param].concat(this.qrcodeList);

    },
    filterQrcodeList(param) {
     const len = this.qrcodeList.length;
     for (let i = 0; i < len; i++) {
       const item = this.qrcodeList[i];
       if(item.title === param.title && item.link === param.link) {
           return item.timestamp;
       }
     }
      return false;
    },
    getDateTimeFromTimestamp(timestamp) {
      if(!timestamp) return '';
      const d = new Date(timestamp);
      return `${d.toLocaleString()}`
    },
    editItemHandle(param) {
      const len = this.qrcodeList.length;
      for (let i=0;i<len;i++) {
        const item = this.qrcodeList[i];
        if (item.timestamp === param.timestamp) {
          this.activeQrcode = item;
          console.log(this.activeQrcode);
        }
      }
    },
    cancelUpdateQrcodeHandle() {
      this.activeQrcode = null;
    },
    updateQrcodeHandle(param) {
      const tmpList = this.qrcodeList;
      const len = tmpList.length;
      for (let i=0;i<len;i++) {
        const item = tmpList[i];
        if(item.title === param.title && item.link === param.link) {
            return alert('Fail: Existed');
        }
        if(item.timestamp === param.timestamp) {
          tmpList[i].title = param.title;
          tmpList[i].link = param.link;
        }
      }
      // update data
      this.qrcodeList = tmpList;
      // reset
      this.activeQrcode = null;
    },
    deleteItemHandle(param) {
      const tmpList = this.qrcodeList;
      const newList = [];
      const len = tmpList.length;
      for (let i=0;i<len;i++) {
        const item = tmpList[i];
        if(item.title === param.title && item.link === param.link && item.timestamp === param.timestamp) {
          continue;
        } else {
          newList.push(item);
        }
      }
      // update data
      this.qrcodeList = newList;
      // reset
      this.activeQrcode = null;
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
