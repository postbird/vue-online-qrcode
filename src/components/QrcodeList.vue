<template>
  <div class="wrapper">
    <div class="row">
      <div class="col-md-3" v-for="(item, index) in list" :key="index">
        <QrcodeItem :item="item" @editItem="editItemHandle" @deleteItem="deleteItemHandle"></QrcodeItem>
      </div>
    </div>
  </div>
</template>

<script>
  import QrcodeItem from './QrcodeItem.vue';
  export default {
    components:{QrcodeItem},
    props: ['list', 'editItem'],
    data() {
      return {

      }
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
</style>