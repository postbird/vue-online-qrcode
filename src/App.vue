<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
        <Header ></Header>
        <Add
            v-if="!activeQrcode"
            @createQrcode="createQrcodeHandle"
            @loadQrcodeList="loadQrcodeListHandle"
        ></Add>
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
        <Draggable v-model="qrcodeList" >
          <transition-group class="row">
            <div class="col-md-3 color-item" v-for="(item, index) in qrcodeList" :key="item.timestamp" >
              <QrcodeItem  :item="item" @editItem="editItemHandle"  @deleteItem="deleteItemHandle"></QrcodeItem>
            </div>
          </transition-group>
        </Draggable>
      </div>
    </div>
  </div>
</template>

<script>
import Add from './components/Add.vue';
import Update from './components/Update.vue';
import Header from './components/Header.vue';
// import QrcodeList from './components/NewQrcodeList.vue';
import QrcodeItem from './components/QrcodeItem';
import Store from 'store2';
import Swal from 'sweetalert2';
import Draggable from 'vuedraggable';

import {QRCODE_STORE_PRE_KEY} from './config/constant';

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
    // QrcodeList,
    Update,
    Draggable,
    QrcodeItem
  },
  created() {

  },
  computed: {
    update() {
      return !!this.activeQrcode;
    }
  },
  updated() {
    this.$nextTick(() => {
      this.saveToStorage();
    })
  },
  methods: {
    // click Create Btn handle
    createQrcodeHandle(param){
      if(!param || !param.title || !param.link) {
          return Swal('Fail：Empty title or empty content');
      }
      const filterTime = this.filterQrcodeList(param);
      if(filterTime) {
        return Swal(`Fail: Existed! Create Time: ${this.getDateTimeFromTimestamp(filterTime)} `);
      }
      // reset
      this.activeQrcode = null;
      this.qrcodeList = [param].concat(this.qrcodeList);
      this.saveToStorage();
    },
    // check isExisted
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
    // format timestamp
    getDateTimeFromTimestamp(timestamp) {
      if(!timestamp) return '';
      const d = new Date(timestamp);
      return `${d.toLocaleString()}`
    },
    // Edit icon click handle
    editItemHandle(param) {
      const len = this.qrcodeList.length;
      for (let i=0;i<len;i++) {
        const item = this.qrcodeList[i];
        if (item.timestamp === param.timestamp) {
          this.activeQrcode = item;
        }
      }
    },
    // Cancel Update click handle
    cancelUpdateQrcodeHandle() {
      this.activeQrcode = null;
    },
    // Update click Handle
    updateQrcodeHandle(param) {
      const tmpList = this.qrcodeList;
      const len = tmpList.length;
      for (let i=0;i<len;i++) {
        const item = tmpList[i];
        if(item.title === param.title && item.link === param.link) {
            return Swal('Fail: Existed');
        }
        if(item.timestamp === param.timestamp) {
          tmpList[i].title = param.title;
          tmpList[i].link = param.link;
        }
      }
      // update data
      this.qrcodeList = tmpList;
      this.saveToStorage();
      // reset
      this.activeQrcode = null;
    },
    // delete Item handle
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
      this.saveToStorage();
      // reset
      this.activeQrcode = null;
    },
    // store list to Storage
    saveToStorage() {
     this.$nextTick(() => {
       // get Date to as key
       const date = new Date();
       const key = QRCODE_STORE_PRE_KEY + date.toLocaleDateString();
       Store.set(key, this.qrcodeList);
      });
    },
    // load list from storage
    loadQrcodeListHandle(datePram) {
      Swal({
          title: 'Are you sure?',
          text: "The operation will replace the current List!",
          type: 'question',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes,Load'
      }).then((result) => {
          if (result.value) {
            console.log(datePram);
            let date = new Date();
            let keyDate = new Date(datePram).toLocaleDateString() || date.toLocaleDateString();
            const key = QRCODE_STORE_PRE_KEY + keyDate;
            const list = Store.get(key);
            if (list) {
              this.qrcodeList = list;
            } else {
              Swal({
                  title: 'List is Empty',
                  type: 'warn',
              });
            }
          }
      })
    },
    // dragend handle
    dragListEndHandle(list) {
      this.qrcodeList = list;
      this.saveToStorage();
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
.color-item {
  transition: transform .3s;
}
</style>
