<template>
  <div class="wrapper">
    <div class="row">
      <div class="col-md-3 color-item" v-for="(item, index) in list" :key="index"v-dragging="{ item: item, list: list, group: 'list' }" >
          <QrcodeItem  :item="item"   @editItem="editItemHandle"  @deleteItem="deleteItemHandle"></QrcodeItem>
      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue';
  import QrcodeItem from './QrcodeItem.vue';
  import VueDND from 'awe-dnd'
  Vue.use(VueDND)
  export default {
    components:{QrcodeItem},
    props: ['list', 'editItem'],
    data() {
      return {

      }
    },
    mounted () {
      this.$dragging.$on('dragged', ({ value }) => {
        console.log(value)
      })
      this.$dragging.$on('dragend', (res) => {
        this.$emit('dragListEnd', this.list)
      })
    },
    methods: {
      editItemHandle(param) {
        this.$emit('editItem', param);
      },
      deleteItemHandle(param) {
        this.$emit('deleteItem', param);
      }
    }
  }
</script>

<style scoped>
.wrapper {
  width: 100%;
}
 .color-item {
    transition: transform .3s;
}
.color-item.dragging {
    transform: scale(1.1);
}

</style>