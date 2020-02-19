<template>
  <div >
    <text class="title" @click="update" :value="date"> </text>
    <selectvue class="list-mask" v-if="isselectaddress" :proviceList="proviceList" :cityListMap="cityListMap" :disListMap="disListMap"  @haveselectedaddress="selectedaddress"></selectvue>
  </div>

</template>

<style>
  .title { font-size: 48px; }
  .list-mask {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    width: 750px;
    z-index: 10;
  }
</style>

<script>
  import selectvue from '@/components/select_address.vue'
  import citydata from "./data/citydataconfig.js";
  export default {
    components:{selectvue},
    methods: {
      update(e) {
        this.isselectaddress = true
      },

      selectedaddress(evtValue){
        this.isselectaddress = false
        if(evtValue === ''){
          return
        }
        this.date = evtValue

      }
    },
    data(){
      return{
        proviceList:[],
        cityListMap:{},
        disListMap:{},
        isselectaddress: false,
        date:'点击选择地址',
        showdisList: true,//显示县区级单位
        renturnMunicipalityThree: true,//直辖市是否显示三级
      }
    },
    mounted(){

      this.proviceList = citydata.proviceList;
      this.cityListMap = citydata.cityListMap;
      this.disListMap = citydata.disListMap;
    }
  }
</script>
