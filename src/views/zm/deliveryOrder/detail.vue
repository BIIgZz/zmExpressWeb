<template>

      <a-drawer
        placement="right"
        width = "1180"
        :closable="false"
        :visible="visibleDetail"
        :after-visible-change="afterVisibleChange"
        @close="onClose"
      >
        <page-layout :title="title" logo="https://gw.alipayobjects.com/zos/rmsportal/nxkuOJlFJuAUhzlMTCEe.png">
          <template slot="action">
            <a-button-group style="margin-right: 4px;">
              <a-button @click='editDelivery' >编辑信息</a-button>
              <a-button>导出运单</a-button>
              <a-button type="danger" >
                取消订单
              </a-button>
              <a-button><a-icon type="ellipsis"/></a-button>
            </a-button-group>
            <a-button type="primary" >操作</a-button>
          </template>
          <a-card :bordered="false">
            <detail-list title="基本信息">
              <detail-list-item term="供应商">{{ deliveryData.supplier }}</detail-list-item>
              <detail-list-item term="参考号">{{ deliveryData.referenceNo }}</detail-list-item>
              <detail-list-item term="发往站点">{{ deliveryData.destination }}</detail-list-item>
              <detail-list-item term="主品名">{{ deliveryData.mainName }}</detail-list-item>
              <detail-list-item term="备注">{{ deliveryData.remark }}</detail-list-item>
              <detail-list-item term="清关、报关">{{ deliveryData.attributes_dictText }}</detail-list-item>
              <detail-list-item term="发出时间">{{ deliveryData.pickingTime }}</detail-list-item>
            </detail-list>
            <a-divider style="margin-bottom: 32px"/>

            <a-row :gutter="24">
              <a-col :span="8">
                <a-card title="运单" style="width: 300px">
                  <a slot="extra" href="#" @click="showWaybills">查看运单</a>
                  <a-statistic title="运单数量"  :value='statistics.waybillCount' style="margin-right: 50px"  :value-style="{ fontSize: '40px' }"  />

                </a-card>
              </a-col>
              <a-col :span="12">
                <a-card title="货箱" style="width: 700px">
                  <a slot="extra"  href="#" @click='showBoxForm' v-if="deliveryData.status==='待出货'">添加货箱   | </a>
                  <a slot="extra" href="#" @click='showCases'>查看货箱</a>
                  <a-row :gutter="12">
                    <a-col :span="6">
                      <a-statistic title="货箱数量" :value='statistics.caseCount' style="margin-right: 50px" :value-style="{ fontSize: '40px' }" />
                    </a-col>
                    <a-col :span="6">
                      <a-statistic title="实重(KG)" :value='statistics.weight' style="margin-right: 50px" :value-style="{ fontSize: '40px' }" />
                    </a-col>
                    <a-col :span="6">
                      <a-statistic title="材积重(KG)" :value='statistics.volumeWeight' style="margin-right: 50px" :value-style="{ fontSize: '40px' }" />
                    </a-col>
                    <a-col :span="6">
                      <a-statistic title="总体积(m³)" :value='statistics.volume' style="margin-right: 50px" :value-style="{ fontSize: '40px' }" />
                    </a-col>
                  </a-row>
                </a-card>
              </a-col>
            </a-row>
            <a-divider style="margin-bottom: 32px"/>
            <detail-list title="用户信息">
              <detail-list-item term="用户姓名">付小小</detail-list-item>
              <detail-list-item term="联系电话">18100000000</detail-list-item>
              <detail-list-item term="常用快递">菜鸟仓储</detail-list-item>
              <detail-list-item term="取货地址">浙江省杭州市西湖区万塘路18号</detail-list-item>
              <detail-list-item term="备注">	无</detail-list-item>
            </detail-list>
            <a-divider style="margin-bottom: 32px"/>
            <boxes-form v-show='visibleBoxesForm'
                        :visibleBoxesForm.sync="visibleBoxesForm"
                        :deliveryData='deliveryData'
                        @getNewStatistics = 'getStatistics'
            ></boxes-form>
          </a-card>

        </page-layout>
        <delivery-and-waybill-list v-show='visibleWaybillList'
                                   :visibleWaybillList.sync="visibleWaybillList"
                                   :deliveryData='deliveryData'
                                   :newStatistics = 'statistics'
                                   @getNewStatistics = 'getStatistics'
        ></delivery-and-waybill-list>
<!--        <delivery-and-case-list v-show='visibleCaseList' :visibleCaseList.sync="visibleCaseList"  :deliveryData='deliveryData'></delivery-and-case-list>-->
        <delivery-and-case-list v-show='visibleCaseList'
                                :visibleCaseList.sync="visibleCaseList"
                                :deliveryData='deliveryData'
                                :newStatistics = 'statistics'
                                @getNewStatistics = 'getStatistics'
        ></delivery-and-case-list>
      </a-drawer>

</template>

<script>
import PageLayout from '@/components/page/PageLayout'
import STable from '@/components/table/'
import DetailList from '@/components/tools/DetailList'
import ABadge from "ant-design-vue/es/badge/Badge"
import { handleDetailss,getAction } from '../../../api/manage'
import DeliveryAndWaybillList from './deliveryAndWaybillList'
import BoxesForm from './boxesForm'
import DeliveryAndCaseList from './deliveryAndCaseList'
const DetailListItem = DetailList.Item

export default {
  name :'zm-delivery-detail',
  components: {
    DeliveryAndCaseList,

    BoxesForm,
    DeliveryAndWaybillList,
    PageLayout,
    ABadge,
    DetailList,
    DetailListItem,
    STable
  },
  props:{
    deliveryData:{
      type:Object,
    },
    visibleDetail:{
      type:Boolean
    },
  },
  data () {
    return {
      title:"",
      statistics:{},
      visibleCaseList:false,
      visibleWaybillList : false,
      visibleBoxesForm : false,
      data:this.deliveryData
    }
  },
  watch:{
    deliveryData:{
      handler(newVal,oldVal){
        if (newVal!=oldVal){
          this.title=newVal.deliveryOrderNo
          this.deliveryData = newVal
          this.data = newVal
          this.getStatistics();
        }
      },
      immediate:true,
    }
  },
  methods:{
    afterVisibleChange(val) {

    },
    showBoxForm:function(){
      this.visibleBoxesForm = true;
    },
    async getStatistics(){
      var list = '/zmexpress/zmDeliveryOrder/statistic'
      var  that = this;
      this.statistics =await handleDetailss(list, { id: this.deliveryData.id}).then(res => {
        if (res.success) {
          return res.result;
        }
      });
    },
    async editDelivery(){
      this.$emit("edit",this.data);
    },
    showWaybills(){

      this.visibleWaybillList = true;
    },
    showCases(){
      this.visibleCaseList = true;
    },
    onClose() {
      this.$emit('update:visibleDetail',false)
      // visible = false;
    },
  },
  computed: {
    // title () {
    //   return this.$route.meta.title
    // }
  },

}
</script>

<style lang="less" scoped>
.title {
  color: rgba(0,0,0,.85);
  font-size: 16px;
  font-weight: 500;
  margin-bottom: 16px;
}
</style>