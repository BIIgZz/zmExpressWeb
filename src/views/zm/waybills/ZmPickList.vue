<template>
  <div>
    <a-drawer
      title="拣货"
      placement="right"
      width = "1080"
      :closable="false"
      :visible="visiblePickList"
      :after-visible-change="afterVisibleChange"
      @close="onClose"
    >

      <div >
        <a-radio-group v-model="value" @change="onChange" style=" padding: 10px">
          <a-radio :value="1" >
            按客户数据拣货
          </a-radio>
          <a-radio :value="2">
            清空所有
          </a-radio>
        </a-radio-group>
        <div style=" padding: 10px">
          重量：<a-input  suffix="KG" v-model="weightInput" style="width: 10%" />
          长：<a-input  suffix="CM" v-model="lengthInput" style="width: 10%" />
          宽：<a-input  suffix="CM" v-model= "widthInput"  style="width: 10%"/>
          高：<a-input   suffix="CM" v-model= "heightInput"  style="width: 10%"/>
          件数：<a-input   suffix="件" v-model= "caseInput"  style="width:10%"/>
          <a-button type="primary" icon="plus" @click='fillTable'>
            填充
          </a-button>
          <a-button type="primary" icon="plus" @click='saveData'>
            保存
          </a-button>
        </div>

      </div>

      <a-form ref="setForm" :model="setForm" >
        <a-form-item label="货箱表" class="hotTable">
          <hot-table id="hot" :settings="goalHotSettings" ref="setExcel" ></hot-table>
        </a-form-item>
      </a-form>
      </a-drawer>
  </div>

</template>
<script>
import { HotTable } from '@handsontable/vue';
import { registerAllModules } from 'handsontable/registry';
import { handleDetailss, putAction } from '../../../api/manage'

// register Handsontable's modules
registerAllModules();

export default {
  name: 'zm-pick-list',
  props: {
    visiblePickList:{
        type:Boolean
      },
    data:{
      type:Object
    },
    waybillStatistic:{
      type:Object
    }
  },
  data() {
    return {
      setForm: {
        period: true,
        object:'',
        targetTableData: [],
        completeTableData: []
      },
      start:0,
      waybillStatistic1:{},
      wayBillData:this.data,
      weightInput:"",
      widthInput:"",
      lengthInput:"",
      heightInput:"",
      caseInput:"",
      volumeWeight:"",
      heavyCharges:"",
      excelData: [],
      value:'2',
      colFlag:0,
      goalHotSettings: {
        data: { },
        columns: [  // 每一列对应的数据和数据类型
          {
            data: 'waybillId',
            type: 'text',
          },
          {
            data: 'caseid',
            type: 'text'
          },

          {
            data: 'weightActually',
            type: 'numeric'
          },
          {
            data: 'lengthActually',
            type: 'numeric'
          },
          {
            data: 'widthActually',
            type: 'numeric'
          },
          {
            data: 'heightActually',
            type: 'numeric'
          },
          {
            data: 'weightVolume',
            type: 'numeric'
          },
          {
            data: 'weightCharge',
            type: 'numeric'
          },

        ],
        colHeaders: [  // 显示列头，为true时显示默认，为数组时，显示数组里面的值
          '运单号/箱号',
          '扩展箱号',
          '重量',
          '长',
          '宽',
          '高',
          '材积重',
          '收费重'
        ],
        // cell: [ ]   // 设置指定单元格的一些属性，比如是否可编辑，定义className
        cells: function(row, column, prop) { // 动态设置一些单元格的属性
          const cellProperties = {readOnly: false, className: ''};
          const visualRowIndex = this.instance.toVisualRow(row);
          const visualColIndex = this.instance.toVisualColumn(column);
          if(visualColIndex === 0 || visualColIndex === 1) {
            cellProperties.readOnly = true;
            cellProperties.className = 'forbidden';
          }
          return cellProperties;
        },
        fixedRowsTop: 1,  // 固定顶部多少行不能垂直滚动
        fixedColumnsLeft: 2, // 固定左侧多少列不能水平滚动
        stretchH: 'all', // last/all/none last为延伸最后一列，all为延伸所有列，none表示默认不延伸
        autoWrapRow: true, // 自动隐藏行
        height: 487, // 高度
        manualRowResize: true, // 允许拖动改变行的高度
        manualColumnResize: true, // 允许拖动改变列的宽度
        rowHeaders: false, // 当值为true时显示行头，当值为数组时，行头为数组的值
        manualRowMove: false, // 为true时，行可拖拽至指定行
        manualColumnMove: false,  // 为true时，列可拖拽至指定列
        contextMenu: true, // 右键菜单展示
        filters: false, // 过滤
        dropdownMenu: false, // 下拉菜单
        selectionMode: "multiple",
        outsideClickDeselects : false,
        licenseKey: "non-commercial-and-evaluation"  // 非商业用途声明
      }
    };
  },

  components: {
    HotTable
  },
  watch: {
    // visible:{
    //   handler(newVal,oldVal){
    //     this.visiblePickList = newVal;
    //   },
    //   immediate:true,
    // },
    visiblePickList:{
      handler(newVal,oldVal){
        if (newVal!=oldVal)
         this.getWayBillDetails();
      },
      immediate:true,
    },
    data:{
      handler(newVal,oldVal){
        if (newVal!=oldVal){
          this.wayBillData = newVal
          this.getWayBillDetails();

        }
      },
      immediate:true,
    },
    waybillStatistic:{
      handler(newVal,oldVal){
        if (newVal!=oldVal){
          this.waybillStatistic1 = newVal
        }
      },
      immediate:true,
    }
  },

  methods: {
    fillTable: function() {
      var flag = 0;
      let newData = this.excelData;
      // if(this.$refs.setExcel.hotInstance.getSelected()){ //hot.getSelected()表示handsontable选择器
      //   var row=this.$refs.setExcel.hotInstance.getSelected()[0]; //表示获取选中的某一行
      //   var column=this.$refs.setExcel.hotInstance.getSelected()[1];//表示获取选中的某一行中的某一行的单元格地址
      //   start = row[0];
      // }
      if (this.start>=newData.length){
        this.$notification.open({
          message: '警告',
          description:
            '数量超过限制',
          onClick: () => {
            console.log('Notification Clicked!');
          },
        });
        return
      }

      let j = 0
      for (let i = 0; i < this.caseInput; i++) {
          j = i+this.start;
          this.colFlag++;
          if (this.widthInput!=''){
            newData[j].widthActually = this.widthInput;
          }else{
            flag = 1;
          }
         if ( this.weightInput!=''){
           newData[j].weightActually = this.weightInput;
         }else {
           flag = 2;
         }
          if (  this.heightInput!=''){
            newData[j].heightActually = this.heightInput;
          }else{
            flag = 3;
          }
          if ( this.lengthInput!=''){
            newData[j].lengthActually = this.lengthInput;
          }else{
            flag = 4;
          }
          if (flag==0){
            //材积重 = 长 * 宽 * 高 / 6000
            //计费重 = 材积重-（材积重 - 实重）* 分泡比例
            let wvData =  (this.lengthInput*this.widthInput*this.heightInput)/(this.waybillStatistic1.foamingFactor);
            //分泡重
            newData[j].weightVolume =wvData.toFixed(1);
             wvData = wvData.toFixed(1);
            if ( parseFloat(wvData) > parseFloat(newData[j].weightActually)){
              //材积重>实重
              if (newData[j].weightActually >= this.waybillStatistic1.minBoxWeight){
                //单箱重＞=最低箱实重
                newData[j].weightCharge = wvData-(wvData-newData[j].weightActually)*this.waybillStatistic.bubbleSplittingRatio;
              }else if (newData[j].weightActually<this.waybillStatistic1.minBoxWeight){
                //单箱重<最低箱实重
                let wcData =  wvData-(wvData-this.waybillStatistic1.minBoxWeight)*this.waybillStatistic.bubbleSplittingRatio;
                console.log(wcData)
                newData[j].weightCharge = wcData ;
              }
            }else if (wvData < newData[j].weightActually){
              //材积重<实重
              console.log("材积重<实重", wvData,newData[j].weightActually)
              if (newData[j].weightActually >= this.waybillStatistic1.minBoxWeight){
                //单箱重 >= 最低箱实重
                newData[j].weightCharge = newData[j].weightActually;
              }else if (newData[j].weightActually < this.waybillStatistic1.minBoxWeight){
                //单箱重 < 最低箱实重
                newData[j].weightCharge = this.waybillStatistic1.minBoxWeight;
              }
            }
          }
        }
      this.start = this.colFlag;
      if (flag>0){
        this.$notification['warning']({
          message: '警告！',
          description:
            '请将长、宽、高、件数填写完整！',
        });
      }else{

        this.$refs.setExcel.hotInstance.loadData(newData);
        this.heightInput='';
        this.weightInput='';
        this.widthInput ='';
        this.lengthInput='';
        this.caseInput = '';
      }

      // this.$refs.hotTableComponent.hotInstance.loadData(this.getWayBillDetails());
    },
    /** 按客户数据拣货*/
    fillTableByUser(){
      let  userData = this.excelData
      for (let i = 0; i < this.excelData.length; i++) {
        userData[i].widthActually = this.excelData[i].width;
        userData[i].weightActually = this.excelData[i].weight;
        userData[i].heightActually =this.excelData[i].height;
        userData[i].lengthActually = this.excelData[i].length;
          let wvData =  ( userData[i].lengthActually* userData[i].heightActually *userData[i].widthActually)/(this.waybillStatistic1.foamingFactor);
        userData[i].weightVolume =wvData.toFixed(2);
        userData[i].weightCharge = ((wvData-this.wayBillData.minBoxWeight)*(1-this.waybillStatistic1.bubbleSplittingRatio)+this.wayBillData.minBoxWeight).toFixed(2);
      }
      this.$refs.setExcel.hotInstance.loadData(userData);
    },
     /** 获取所有可用箱子数据*/
    async getWayBillDetails(){
      var list = '/zmexpress/zmWaybills/queryZmGoodCaseByMainId'
      var  that = this;
      this.excelData =await handleDetailss(list, { id: this.wayBillData.id}).then(res => {
        if (res.success) {
          this.$refs.setExcel.hotInstance.loadData( res.result.records);
          return res.result.records;
        }
      });
    },
    afterVisibleChange(val) {
    },
    showDrawer() {
      this.getWayBillDetails();
    },
    onChange(e) {
      if (e.target.value=="2"){
        let newData = this.excelData;
        for (let i = 0; i < this.excelData.length; i++) {
          newData[i].widthActually = 0;
          newData[i].weightActually = 0;
          newData[i].heightActually = 0;
          newData[i].lengthActually = 0;
          newData[i].weightVolume = 0;
          newData[i].weightCharge = 0;
        }
        this.$refs.setExcel.hotInstance.loadData(newData);
      }else if (e.target.value == "1"){
        this.fillTableByUser();
      }

    },
    onClose() {
      this.$emit('update:visiblePickList',false)
      // visible = false;
    },
    saveData (){
      let newData = this.$refs.setExcel.hotInstance.getData();
      for (let i = 0; i < this.excelData.length; i++) {
          this.excelData[1].weightActually = newData[i][2];
          this.excelData[1].widthActually = newData[i][4];
          this.excelData[1].heightActually = newData[i][5];
          this.excelData[1].lengthActually = newData[i][3];
      }
      let that = this;
      this.$confirm({
        title: "确认保存？",
        content: "是否保存修改的数据?",
        onOk: function() {
          putAction(  '/zmexpress/zmGoodCase/updateData',that.excelData ).then((res) => {
            if (res.success) {
              that.$message.success(res.message);
              that.$emit('fatherFn')
              that.$emit('newStatistic')
              that.$emit('newStatus',"已收货")
              that.start=0
              that.$forceUpdate()
            } else {
              that.$message.warning(res.message);
            }
          }).finally(() => {
          });
        }
      });
      // setTimeout(()=>{   //设置延迟执行
      //   this.visiblePickList = false;
      // },2000);
    }
  },
}
</script>

<style src="../../../../node_modules/handsontable/dist/handsontable.full.css"></style>