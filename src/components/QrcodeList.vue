<template>
  <div class="wrapper">
    <div class="row">
      <div class="col-md-3" v-for="(item, index) in list" :key="index">
        <QrcodeItem :item="item"></QrcodeItem>
      </div>
    </div>
  </div>
</template>

<script>
  import QrcodeItem from './QrcodeItem.vue';
  export default {
    components:{QrcodeItem},
    props: ['list'],
    data() {
      return {

      }
    }
  }
</script>

<style scoped>
.wrapper {
  width: 100%;

}
</style>