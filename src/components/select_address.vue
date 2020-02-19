<template>
    <div class="stories-view" append="tree"  >
        <div class="list-mask" :style="{height:`${totalheight-80}px`}"  @click="unselectedaddress"></div>
        <text class="addbutton basebutton" @click="selectedaddress" value="确定"></text>
        <div class="select-item"  >
            <list class="listitem" >
                <cell v-for="(item, index) in proviceList" append="tree" @click="selectprovince(index)">
                    <text class="cityitem" :style="{color:(index === selectindex)?'#3399ff':'gray'}"> {{item.name}} </text>
                </cell>
            </list>
            <list class="listitem" >
                <cell v-for="(item ,index) in cityList" append="tree" @click="selectcity(index)">
                    <text class="cityitem" :style="{color:(index === selectcityindex)?'#3399ff':'gray'}"> {{item.name}} </text>
                </cell>
            </list>
            <list class="listitem"  v-if="showdisList" >
                <cell v-for="(item, index) in disList" append="tree" @click="selectdist(index)" >
                    <text class="cityitem" :style="{color:(index === selectdisindex)?'#3399ff':'gray'}"> {{item.name}} </text>
                </cell>
            </list>
        </div>
    </div>
</template>

<style>
    .stories-view {
        min-height:250px;
        overflow-y:auto;

    }
    .list-mask{
        position: absolute;
        top: 0;
        left: 0;
        width: 750px;
        z-index: 10;
        opacity: 0.65;
        background-color: gray;
    }
    .select-item{
        flex-direction: row;
        flex-wrap: nowrap;
        position: absolute;
        background-color: white;
        align-items: center;
        justify-content: center;
        bottom: 80px;
        height: 600px;
        width: 750px;
        z-index:101;
        opacity: 1;
        background-color: white;
    }
    .listitem{
        max-height: 500px;
        margin-top: 20px;
        margin-bottom: 20px;
        width: 250px;
        max-height: 500px;
        flex-grow:1;
        opacity: 1;
        background-color: white;
    }
    .cityitem{
        color: gray;
        text-align: center;
        padding-top: 10px;
        padding-bottom: 10px;
        font-size: 32px;
    }
    .addbutton{
        bottom: 0px;
        width:750px;
        padding-top: 18px;
        text-align: center;
    }
    .basebutton{
        color:white;
        background-color: ;
        position: absolute;
        font-size:32px;
        height:80px;
    }
</style>

<script>
    export default {
        props: {
            proviceList: {
                type: Array,
                required: true
            },
            cityListMap: {
                type: Object,
                required: true
            },
            disListMap: {
                type: Object,
                required: true
            },
            //是否显示 县区级行政
            showdisList: {
                type: Boolean,
                default: true,
            },
            //返回直辖市 北京-北京-东城 这样格式 省-市 则是 北京一个
            renturnMunicipalityThree: {
                type: Boolean,
                default: true,
            },

        },
        data() {
            return{
                selectindex:0,
                selectcityindex:0,
                selectdisindex:0,
                cityList:[],//当前市列表
                disList:[],  // 当前区列表
                selectedprovince:{},
                selectedcity:{},
                selecteddist:{},
                selectplace:'',
            }
        },
        methods: {
            selectedaddress(){
                //  this.isselectaddress = false  //关闭选择框
                //先判断是否直辖市返回三级
                //有些市无下辖区县 要做判断
                if(this.renturnMunicipalityThree){
                    if (this.showdisList&&this.disList!=null&&this.disList.length>this.selectdisindex){
                        this.selectplace =  this.proviceList[this.selectindex].name+'  '+ this.cityList[this.selectcityindex].name +'  ' + this.disList[this.selectdisindex].name;

                    } else {
                        this.selectplace =  this.proviceList[this.selectindex].name+'  '+ this.cityList[this.selectcityindex].name;

                    }
                }else{
                    //祛除直辖市 的省级 name拼接
                    if (this.showdisList&&this.disList!=null&&this.disList.length>this.selectdisindex){
                        //索引 0 12 3
                        if(this.selectindex<4){
                            this.selectplace =    this.cityList[this.selectcityindex].name +'  ' + this.disList[this.selectdisindex].name;

                        }  else{
                            this.selectplace =  this.proviceList[this.selectindex].name+'  '+ this.cityList[this.selectcityindex].name +'  ' + this.disList[this.selectdisindex].name;

                        }

                    } else {
                        if(this.selectindex<4){
                            this.selectplace =    this.cityList[this.selectcityindex].name
                        }  else{
                            this.selectplace =  this.proviceList[this.selectindex].name+'  '+ this.cityList[this.selectcityindex].name;
                        }

                    }


                }

                this.$emit('haveselectedaddress',this.selectplace);

            },

            unselectedaddress(){
                this.$emit('haveselectedaddress','');
            },
            selectprovince(index){
                this.selectedprovince = this.proviceList[index];
                //显示 市和区
                this.cityList = this.cityListMap[this.proviceList[index].recordId];
                this.disList = this.disListMap[this.cityList[0].recordId];
                this.selectindex = index;
                this.selectcityindex = 0;
                this.selectdisindex =0;
            },
            selectcity(index){
                this.selectedcity = this.cityList[index];
                //显示区
                this.disList = this.disListMap[this.cityList[index].recordId];
                this.selectcityindex = index;
                this.selectdisindex =0;
            },
            selectdist(index){
                this.selecteddist = this.disList[index];
                this.selectdisindex = index;
            }
        },
        computed: {
            totalheight(){
                const height = 750/weex.config.env.deviceWidth*weex.config.env.deviceHeight

                return height
            }
        },
        created(){
            this.cityList =this.cityListMap[this.proviceList[this.selectindex].recordId]
            this.disList = this.disListMap[this.cityList[this.selectcityindex].recordId]
        }
    }
</script>
