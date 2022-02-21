<template xmlns:height='http://www.w3.org/1999/xhtml'>
  <page-layout :title="pageTitle" logo="https://gw.alipayobjects.com/zos/rmsportal/nxkuOJlFJuAUhzlMTCEe.png">
    <detail-list slot="headerContent" size="small" :col="2" class="detail-layout">

      <detail-list-item term="客户">{{myProp.username}}</detail-list-item>
      <detail-list-item term="目的地">{{ myProp.destination}}</detail-list-item>
      <detail-list-item term="发往国家" >{{myProp.recipientCountry}}</detail-list-item>
      <detail-list-item term="服务类型">{{ myProp.service}}</detail-list-item>
      <detail-list-item term="交货条款"></detail-list-item>
      <detail-list-item term="创建时间">{{ myProp.createTime }}</detail-list-item>
      <detail-list-item term="FBA号"><a>{{ myProp.fbaId }}</a></detail-list-item>
      <detail-list-item term="创建日期">{{ myProp.createTime }}</detail-list-item>
      <detail-list-item term="备注" >{{myProp.remark}}</detail-list-item>
    </detail-list>
    <a-row slot="extra" class="status-list">
      <a-col :xs="12" :sm="12">
        <div class="text">状态</div>
        <div class="heading" >{{ status }}</div>

      </a-col>
      <a-col :xs="12" :sm="12">
        <div class="text">费用</div>
        <div class="heading">¥  568.08 </div>
      </a-col>
    </a-row>
    <!-- actions -->
    <template slot="action">
      <a-button-group style="margin-right: 4px;">
        <a-button @click='showPickList'>拣货</a-button>
        <a-button>导出运单</a-button>
        <a-button type="danger" @click='cancel' >
          取消订单
        </a-button>
        <a-button><a-icon type="ellipsis"/></a-button>
      </a-button-group>
      <a-button type="primary" >主操作</a-button>
    </template>
    <div style=" padding: 30px">
      <a-card>
        <a-row :gutter="24">
          <a-col :span="4">
            <a-statistic title="收费重" suffix="kg" :value='waybillStatistics.weightCharge' style="margin-right: 20px" :value-style="{ fontWeight:'bond',color:' #004d00',fontSize: '30px' }" />
          </a-col>
          <a-col :span="4">
            <a-statistic title="实重(KG)" suffix="kg" :value='waybillStatistics.weight' style="margin-right: 20px" :value-style="{color:' #004d00', fontSize: '30px' }" />
          </a-col>
          <a-col :span="4">
            <a-statistic title="材积重(KG)" suffix="kg" :value='waybillStatistics.volumnWeight' style="margin-right: 20px" :value-style="{ color:' #004d00',fontSize: '30px' }" />
          </a-col>
          <a-col :span="4">
            <a-statistic title="总体积(m³)" suffix="m³" :value='waybillStatistics.volume' style="margin-right: 20px" :value-style="{ color:' #004d00',fontSize: '30px' }" />
          </a-col>
          <a-col :span="4">
            <a-statistic title="箱数" suffix="箱" :value='waybillStatistics.pieces' style="margin-right: 20px" :value-style="{ color:' #004d00',fontSize: '30px' }" />
          </a-col>
          <a-col :span="4">
            <a-statistic title="计泡系数" :value='waybillStatistics.foamingFactor' style="margin-right: 20px" :value-style="{ color:' #004d00',fontSize: '30px' }" />
          </a-col>

        </a-row>
      </a-card>
    </div>
      <div >
        <a-row :gutter='24'>
          <a-col :span="16">
            <a-collapse default-active-key="1" :bordered="false">
              <a-collapse-panel key="1" header="基础信息">
                <detail-list>

                  <!--        <detail-list-item term="收件人">{{myProp.recipient}}<br>{{warehouseDetail.address1}}</detail-list-item>-->
                  <detail-list-item term="发件人">{{myProp.username}}</detail-list-item>
                  <detail-list-item term="地址">{{myProp.recipientAddressOne}}</detail-list-item>
                  <!--        <detail-list-item term="身份证">{{myProp.recipient}}</detail-list-item>-->
                  <detail-list-item term="报关方式">{{myProp.customsDeclaration_dictText}}</detail-list-item>
                  <detail-list-item term="清关方式">{{myProp.customsClearance_dictText}}</detail-list-item>
                  <detail-list-item term="交税方式">{{myProp.taxPayment_dictText}}</detail-list-item>
                  <detail-list-item term="交货条款">{{myProp.termsDelivery_dictText}}</detail-list-item>
                  <!--        <detail-list-item term="申报币种">{{myProp.recipient}}</detail-list-item>-->
                  <detail-list-item term="客户单号">{{myProp.waybillId}}</detail-list-item>
                  <detail-list-item term="Amazon Reference ID">{{myProp.amazonReferenceId}}</detail-list-item>
                  <detail-list-item term="扩展单号">{{myProp.waybillId}}</detail-list-item>
                  <detail-list-item term="属性">{{myProp.itemProperties}}</detail-list-item>
                  <detail-list-item term="计费时间">{{myProp.createTime}}</detail-list-item>

                </detail-list>
              </a-collapse-panel>
            </a-collapse>
          </a-col>
          <a-col :span="8">
            <a-collapse default-active-key="3" :bordered="false">
              <a-collapse-panel key="3" header="路由信息">
              <a-timeline  mode="alternate" >
                <a-timeline-item v-for="(item,index) in  logDetail"  :key='index'>
                  <a-icon slot="dot" type="down-circle" style="font-size: 16px;"  @click='deleteMsg(item.id)'/>
                  {{ item.msg }}
                  <label v-if='item.remark!=""'><br/>备注：{{item.remark}}</label>
                  <br/>{{ item.createTime }}
                </a-timeline-item>

                <a-tooltip placement="bottom" >
                  <template slot="title"  >
                    添加路由信息
                  </template>
                  <a-timeline-item color="red">
                    <a-icon slot="dot" type="clock-circle-o" style="font-size: 16px;"  @click='showSubmitMsg'/>
                  </a-timeline-item>
                </a-tooltip>
              </a-timeline>
              </a-collapse-panel>
            </a-collapse>
          </a-col>
        </a-row>
      </div>
    <!-- 添加路由信息-->
    <div>
      <a-modal v-model="visibleLogistics" title="路由信息"  @ok="submitMsg">
        <a-form-model ref="form" :model="formData" >
          <!--  JDate -->
          <a-row  >
            <a-col   >
              <a-form-model-item label="时间" >
                <j-date v-model="formData.date" :showTime="true" dateFormat="YYYY-MM-DD HH:mm:ss"/>
              </a-form-model-item>
            </a-col>
          </a-row>
          <!--  操作信息 -->
          <a-row >
            <a-col>
              <a-form-model-item label="操作信息" >
                <a-input  v-model="formData.msg" placeholder="请输入操作信息" />
              </a-form-model-item>
            </a-col>
          </a-row>

          <!--  备注 -->
          <a-row  >
            <a-col  :md="24" :sm="24">
              <a-form-model-item label="备注" >
                <a-textarea
                  v-model="formData.remark"
                  placeholder="请输入备注"
                  :auto-size="{ minRows: 3, maxRows: 5 }"
                />
              </a-form-model-item>
            </a-col>
          </a-row>

        </a-form-model>
      </a-modal>
    </div>

    <!-- 操作 -->
    <a-card
      style="margin-top: 24px"
      :bordered="false"
      :tabList="tabList"
      :activeTabKey="activeTabKey"
      @tabChange="key => onTabChange(key)"

    >
      <div>
        <div v-if="activeTabKey === '1'" style="margin: 5px " >
          <a-button    @click='showPickList' >拣货</a-button>
        </div>


        <a-table
          v-if="activeTabKey === '1'"
          :rowKey="(record,index)=>{return index}"
          :columns="caseColumn"
          :dataSource="caseList"
          :pagination="false"
          :scroll="{ x: 1000, y: 500 }"
          size='small'

        >
          <template
            slot="status"
            slot-scope="status">
            <a-badge :status="status | statusTypeFilter" :text="status | statusFilter"/>
          </template>
        </a-table>
      </div>

      <a-table
        v-if="activeTabKey === '2'"
        :columns="kindColumns"
        :rowKey="(record,index)=>{return index}"
        :dataSource="kindList"
        :pagination="false"
        size='small'
        :scroll="{ x: 1000, y: 500 }"
      >
        <span slot="pic" slot-scope="text, record">
         <img style="width:50px;heigth:50px" :src="record.picture" />
        </span>
        <template
          slot="status"
          slot-scope="status">
          <a-badge :status="status | statusTypeFilter" :text="status | statusFilter"/>
        </template>
      </a-table>
      <a-table
        v-if="activeTabKey === '3'"
        :columns="packingListColumns"
        :rowKey="(record,index)=>{return index}"
        :dataSource="packList"
        :pagination="false"
        size = 'small'
      >
        <span slot="pic" slot-scope="text, record">
         <img style="width:50px;heigth:50px" :src="record.picture" />
        </span>

      </a-table>
      <div  v-if="activeTabKey === '4'">
        <div  style="margin: 5px " >

          <a-button   @click='handleImportFile' icon="import">上传附件</a-button>

        <import-file ref="ImportFile"
                     :url="getImportUrl()"
                     :importId='myProp.id'
                     @ok="importOk">
        </import-file>
        </div>
        <a-modal v-model="visibleRename" title="修改文件名称" @ok="handleRenameOk">
          <a-input placeholder="请输入新文件名称"  v-model='newName'/>
        </a-modal>
        <a-list
          class="demo-loadmore-list"
          :loading="loading"
          item-layout="horizontal"
          :data-source="fileList"
        >

          <a-list-item slot="renderItem" slot-scope="item, index">
            <a slot="actions" @click='showRenameModal(item)'>编辑</a>
            <a slot="actions" @click='downloadFile(item.fileName)'>下载</a>
            <a slot="actions" @click='deleteFile(item.id)' >删除</a>


            <a-tooltip placement="topLeft" title="下载文件">
              <a-list-item-meta
                :description="item.fileName"
                @click='downloadFile(item.fileName)'
              >
              </a-list-item-meta>
            </a-tooltip>
            <div>{{item.createBy}}</div>&nbsp&nbsp&nbsp
            <div>{{item.createTime}}</div>
          </a-list-item>
        </a-list>
      </div>
      <div v-if="activeTabKey==='5'">
        <a-table :columns="operateColumns" :data-source="operateData">
        </a-table>
      </div>



    </a-card>
    <zm-pick-list v-show='visiblePickList'
                  :visiblePickList.sync="visiblePickList"
                  :data="myProp"
                  :waybillStatistic='waybillStatistics'
                  @fatherFn="fatherFnTwo"
                  @newStatistic = 'getStatistics'
                  @newStatus='changeStatus'
    ></zm-pick-list>
  </page-layout>
</template>

<script>
  import { mixinDevice } from '@/utils/mixin.js'
  import PageLayout from '@/components/page/PageLayout'
  import DetailList from '@/components/tools/DetailList'
  import { handleDetailss, putAction, deleteAction, downloadFile, downFile,postAction } from '../../../api/manage'
  import { JeecgListMixin } from '../../../mixins/JeecgListMixin'
  import JDate from '@/components/jeecg/JDate'
  import ZmPickList from './ZmPickList'
  import importFile from './importFile'
  import Vue from 'vue'
  const DetailListItem = DetailList.Item

  export default {
    name: "Advanced",
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      ZmPickList,
      PageLayout,
      DetailList,
      DetailListItem,
      importFile
    },
    props:{
      myProp:{
        type:Object,
        default:()=>{}
      }
    },
    data () {
      return {
        operateData:[],
        operateColumns:[
          {
            title: '操作内容',
            dataIndex: 'logContent',
            key: 'logContent',
          },
          {
            title: '操作人',
            dataIndex: 'username',
            key: 'username',
          },
          {
            title: '时间',
            dataIndex: 'createTime',
            key: 'createTime',
          },
        ],
        disableMixinCreated:true,
        visibleRename:false,
        itemFile:{},
        newName:'',
        pageTitle:this.myProp.orderId+'/'+this.myProp.waybillId+'/'+this.myProp.username,
        data:this.myProp,
        status:"",
        warehouse:this.myProp.warehouseId,
        serviceList:"",
        caseList:[],
        kindList:[],
        packList:[],
        fileList:[],
        waybillStatistics: {},
        warehouseDetail:"",
        //控制拣货显示
        visiblePickList:false,
        logDetail :"",
        //控制添加物流表单
        visibleLogistics: false,
        tabList: [
          {
            key: '1',
            tab: '货箱信息'
          },
          {
            key: '2',
            tab: '品名'
          },
          {
            key: '3',
            tab: '装箱单'
          },
          {
            key: '4',
            tab: '附件'
          },
          {
            key: '5',
            tab: '操作日志'
          }
        ],
        activeTabKey: '1',
        formData: {
          date: this.getCurrentTime(),
          remark:'',
          msg:'',
          orderId:'',
        },
        caseColumn:[
          {
            title: '货箱号',
            dataIndex: 'caseId',
            key: 'caseId',
            customRender: (text, record, index) => {
              // 例如：text = [1, 2, 3, 4]
              const textArr = text.split('</br>')
              return (<div>
                {
                  textArr.map(t => {
                    return (<li>{t}</li>)
                  })
                }
              </div>)
            }
          },
          {
            title: '客户数据',
            dataIndex: 'customerData',
            key: 'customerData',
            customRender: (text, record, index) => {
              // 例如：text = [1, 2, 3, 4]
              const textArr = text.split('</br>')
              return (<div>
                {
                  textArr.map(t => {
                    return (<li>{t}</li>)
                  })
                }
              </div>)
            }
          },
          {
            title: '拣货数据(实重/材重)',
            dataIndex: 'pickData',
            key: 'pickData',
            customRender: (text, record, index) => {
              // 例如：text = [1, 2, 3, 4]
              const textArr = text.split('</br>')
              return (<div>
                {
                  textArr.map(t => {
                    return (<li>{t}</li>)
                  })
                }
              </div>)
            }
          },
          {
            title: '提单号',
            dataIndex: 'ladingId',
            key: 'ladingId'
          },
          {
            title: '承运商',
            dataIndex: 'carrier',
            key: 'carrier',
            customRender: (text, record, index) => {
              // 例如：text = [1, 2, 3, 4]
              const textArr = text.split('</br>')
              return (<div>
                {
                  textArr.map(t => {
                    return (<li>{t}</li>)
                  })
                }
              </div>)
            }
          },
        ],
        kindColumns: [
          {
            title: '商品SKU',
            dataIndex: 'sku',
            key: 'sku'
          },
          {
            title: 'PO Number',
            dataIndex: 'po',
            key: 'po'
          },
          {
            title: '中文品名',
            dataIndex: 'cnName',
            key: 'cnName',
          },
          {
            title: '英文品名',
            dataIndex: 'enName',
            key: 'enName'
          },
          {
            title: '申报单价',
            dataIndex: 'declaredPrice',
            key: 'declaredPrice'
          },
          {
            title: '数量',
            dataIndex: 'declaredNumber',
            key: 'declaredNumber'
          },
          {
            title: '材质',
            dataIndex: 'material',
            key: 'material'
          },
          {
            title: '用途',
            dataIndex: 'application',
            key: 'application'
          },
          {
            title: '品牌',
            dataIndex: 'brand',
            key: 'brand'
          },
          {
            title: '品牌类型',
            dataIndex: 'type',
            key: 'type'
          },
          {
            title: '型号',
            dataIndex: 'model',
            key: 'model'
          },
          {
            title: '图片链接',
            dataIndex: 'picture',
            key: 'picture',
            scopedSlots: { customRender: 'pic' }
          },
          {
            title: '海关编码',
            dataIndex: 'hscode',
            key: 'hscode'
          },
          {
            title: '产品带电',
            dataIndex: 'electric',
            key: 'electric'
          },
          {
            title: '产品带磁',
            dataIndex: 'magnetic',
            key: 'magnetic'
          },
        ],

        packingListColumns: [
          {
            title: '货箱编号',
            dataIndex: 'caseId',
            key: 'caseId',
            width: 180,
            customRender: (text, record, index) => {
              // 例如：text = [1, 2, 3, 4]
              const textArr = text.split('</br>')
              return (<div>
                {
                  textArr.map(t => {
                    return (<li>{t}</li>)
                  })
                }
              </div>)
            }
          },
          {
            title: '商品SKU',
            dataIndex: 'sku',
            key: 'sku'
          },
          {
            title: 'PO Number',
            dataIndex: 'po',
            key: 'po'
          },
          {
            title: '中文品名',
            dataIndex: 'cnName',
            key: 'cnName',
          },
          {
            title: '英文品名',
            dataIndex: 'enName',
            key: 'enName'
          },
          {
            title: '申报单价',
            dataIndex: 'declaredPrice',
            key: 'declaredPrice'
          },
          {
            title: '数量',
            dataIndex: 'declaredNumber',
            key: 'declaredNumber'
          },
          {
            title: '材质',
            dataIndex: 'material',
            key: 'material'
          },
          {
            title: '用途',
            dataIndex: 'application',
            key: 'application'
          },
          {
            title: '品牌',
            dataIndex: 'brand',
            key: 'brand'
          },
          {
            title: '品牌类型',
            dataIndex: 'type',
            key: 'type'
          },
          {
            title: '型号',
            dataIndex: 'model',
            key: 'model'
          },
          {
            title: '图片链接',
            dataIndex: 'picture',
            key: 'picture',
            scopedSlots: { customRender: 'pic' }
          },
          {
            title: '海关编码',
            dataIndex: 'hscode',
            key: 'hscode'
          },
          {
            title: '产品带电',
            dataIndex: 'electric',
            key: 'electric'
          },
          {
            title: '产品带磁',
            dataIndex: 'magnetic',
            key: 'magnetic'
          },
        ],
      }
    },
    watch:{
      myProp: {
        handler (newVal,oldVal) {
            this.pageTitle=newVal.orderId+'/'+newVal.waybillId+'/'+newVal.username,
            this.myProp=newVal;
            this.status = this.myProp.status;
            this.getLogDetails();
            this.getCaseDetails();
            this.getKindDetails();
            this.getPackListByWayBillId();
            this.getStatistics();
            this.getOperateLog();
        },
        immediate:true
      },
    },
    created() {
      this.getLogDetails();
    },
    mounted:function(){
      // this.getServiceDetails(this.service);
      // this.getWarehouseDetails(this.warehouse);
    },
    methods:{
      importOk(){
        this.getFileList();
      },
      handleImportFile(){
        this.$refs.ImportFile.show()
      },
      getImportUrl(){
        return '/zmexpress/zmFilePath/importExcel'
      },

      showPickList(){
        this.visiblePickList = true
      },
      cancel: function() {
          var ids = this.myProp.id;
          var that = this;
          this.$confirm({
            title: "确认取消？",
            content: "是否取消该订单?",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmWaybills/cancel',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
      },
      fatherFnTwo(){
        this.$emit('fatherFn')
      },
      async getServiceDetails(record){
        let params = record;
        this.serviceList =await handleDetailss( '/zmexpress/zmService/queryByName', {name: params}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },
      changeStatus(val){
        this.status=val;
      },
      /** 获取运单统计*/
      async getStatistics(){
        this.waybillStatistics =await handleDetailss( '/zmexpress/zmWaybills/statisticsById', {id: this.myProp.id}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },
      /** 获取装箱列表*/
      async getPackListByWayBillId(){
        this.packList =await handleDetailss( '/zmexpress/zmGoodCase/getPackListByWayBillId', {id: this.myProp.id}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },
      /** 获取货箱列表*/
      async getCaseDetails(){
        this.caseList =await handleDetailss( '/zmexpress/zmGoodCase/getCaseListByWayBillId', {id: this.myProp.id}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },
      /** 获取货箱归类列表*/
      async getKindDetails(){
        this.kindList =await handleDetailss( '/zmexpress/zmGoodCase/getKindListByWayBillId', {id: this.myProp.id}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },
      /** 获取附件列表*/
      async getFileList(){
        this.fileList =await handleDetailss( '/zmexpress/zmFilePath/queryByWaybillId', {id: this.myProp.id}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },
      /** 重命名文件*/
      handleRenameOk(){
        var  that = this;
        this.itemFile.fileName = this.newName;
        putAction(  '/zmexpress/zmFilePath/editFileName',this.itemFile).then((res) => {
          if (res.success) {
            that.$message.success(res.message);
            that.getFileList();
          } else {
            that.$message.warning(res.message);
          }
        }).finally(() => {
          that.loading = false;
        });
        this.visibleRename = false;
      },
      showRenameModal(item) {
        this.itemFile=item;
        this.newName=item.fileName;
        this.visibleRename = true;
      },
      /** 删除文件*/
      deleteFile(id){
        var  that = this;
        this.$confirm({
            title: '警告',
            content: '真的要删除吗?',
            okText: '删除',
            okType: 'danger',
            cancelText: '取消',
            onOk() {
              deleteAction(  '/zmexpress/zmFilePath/delete',{id:id}).then((res) => {
                if (res.success) {
                  that.getFileList();
                  that.$message.success(res.message);
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            },
            onCancel() {

            },
        });
      },
      /** tab切换事件*/
      onTabChange(key){
        this.activeTabKey = key
          if (key==4){
            this.getFileList();
          }
      },

      /** 添加物流信息 */
      showSubmitMsg(){
        this.visibleLogistics=true;
      },
      /** 添加物流信息*/
      submitMsg: function() {
        var  myDate  = new Date();
        var  that = this;
        this.formData.orderId=this.myProp.orderId;
        putAction(  '/zmexpress/zmLogisticsInformation/addMsg',this.formData).then((res) => {
          if (res.success) {
            that.$message.success(res.message);
            that.getLogDetails();
          } else {
            that.$message.warning(res.message);
          }
        }).finally(() => {
          that.loading = false;
        });
        setTimeout(() => {
          this.visibleLogistics = false;
        }, 20);
        this.formData={
          date: this.getCurrentTime(),
          remark:'',
          msg:'',
          orderId: this.myProp.id,
        };
      },
      /** 删除物流信息*/
      deleteMsg: function(id) {
        var  myDate  = new Date();
        var  that = this;
        this.$confirm({
          title: '警告',
          content: '真的要删除吗?',
          okText: '删除',
          okType: 'danger',
          cancelText: '取消',
          onOk() {
            deleteAction(  '/zmexpress/zmLogisticsInformation/delete',{id:id}).then((res) => {
              if (res.success) {
                that.getLogDetails();
                that.$message.success(res.message);
              } else {
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.loading = false;
            });
          },
          onCancel() {

          },
        });

      },

      async getLogDetails(){
        let params = this.myProp.orderId;
        this.logDetail =await handleDetailss( '/zmexpress/zmLogisticsInformation/queryByWaybillId', {waybillId: params}).then(res => {
          if (res.success) {
            return res.result;
          }
        });

      },

      async getWarehouseDetails(record){
        let params = record;
        this.warehouseDetail =await handleDetailss('/zmexpress/zmFbaWarehouse/listZmFbaWarehouseDetailById', {code: params}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
        if (this.warehouseDetail!=null){
          this.myProp.recipient=this.warehouseDetail.linkman;
        }
      },
      /** 获取提单信息*/
      async getWayDetails(id){
        this.testData =await handleDetailss( '/zmexpress/zmBillloading/queryById', { id:id }).then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 获取操作日志*/
      async getOperateLog(){
        let params = this.myProp.id;
        this.operateData =await handleDetailss( '/sys/log/query', {id: params}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
      },

    },

    filters: {
      statusFilter(status) {
        const statusMap = {
          'agree': '成功',
          'reject': '驳回'
        }
        return statusMap[status]
      },
      statusTypeFilter(type) {
        const statusTypeMap = {
          'agree': 'success',
          'reject': 'error'
        }
        return statusTypeMap[type]
      }
    }
  }
</script>

<style lang="less" scoped>
  .demo-loadmore-list {
    min-height: 350px;
  }
  .detail-layout {
    margin-left: 44px;
  }
  .text {
    color: rgba(0, 0, 0, .45);
  }

  .heading {
    color: rgba(0, 0, 0, .85);
    font-size: 20px;
  }

  .no-data {
    color: rgba(0, 0, 0, .25);
    text-align: center;
    line-height: 64px;
    font-size: 16px;

    i {
      font-size: 24px;
      margin-right: 16px;
      position: relative;
      top: 3px;
    }
  }

  .mobile {
    .detail-layout {
      margin-left: unset;
    }
    .text {

    }
    .status-list {
      text-align: left;
    }
  }
</style>