<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="出货单号">
              <a-input placeholder="请输入出货单号" v-model="queryParam.deliveryOrderNo"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="发往站点">
              <j-search-select-tag placeholder="请选择发往站点" v-model="queryParam.destination" dict="zm_airport,name,name"/>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="主品名">
                <a-input placeholder="请输入主品名" v-model="queryParam.mainName"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="供应商">
                <j-dict-select-tag placeholder="请选择供应商" v-model="queryParam.supplier" dictCode="zm_supplier,company,company"/>
              </a-form-item>
            </a-col>
            <a-col :xl="10" :lg="11" :md="12" :sm="24">
              <a-form-item label="出货时间">
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择开始时间" class="query-group-cust" v-model="queryParam.pickingTime_begin"></j-date>
                <span class="query-group-split-cust"></span>
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择结束时间" class="query-group-cust" v-model="queryParam.pickingTime_end"></j-date>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="发出站点">
                <j-search-select-tag placeholder="请选择发出站点" v-model="queryParam.departure" dict="zm_airport,name,name"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="参考号">
                <a-input placeholder="请输入参考号" v-model="queryParam.referenceNo"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="属性">
                <j-dict-select-tag placeholder="请选择属性" v-model="queryParam.attributes" dictCode="delivery_order_item"/>
              </a-form-item>
            </a-col>
          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('出货单')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-tabs type="card"  @change="searchQuery"  v-model="queryParam.status">
        <a-tab-pane key="待出货" :tab="sortNum[0]">
          <a-button-group >
            <a-button icon="form" @click="showAddToBill">放入提单</a-button>
            <a-button icon="car" @click="transfer">出站</a-button>
            <a-button icon="reload">更新统计</a-button>
            <a-button type="danger" @click="cancel" key='0' > <a-icon type="close" />删除</a-button>
          </a-button-group>
        </a-tab-pane>
        <a-tab-pane key="已出货" :tab="sortNum[1]">
        </a-tab-pane>
        <a-tab-pane key="" :tab="sortNum[2]">
        </a-tab-pane>
      </a-tabs>
      <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        class="j-table-force-nowrap"
        @change="handleTableChange">

        <a slot="httpurl" slot-scope="text">{{ text }}</a>
        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="downloadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
      <!--加入提单-->
      <div>
        <a-modal v-model="visibleAddtoBill" title="确认提单"  @ok="submitAddtoBill">
          <a-form-model ref="form" :model="formData" >
            <!--  JDate -->
            <a-row  >
              <a-col   >
                <a-form-model-item label="柜号"  >
                  <a-select
                    v-decorator="[
                      'select',
                      { rules: [{ required: true, message: '请选择提单！' }] },
                  ]"
                    placeholder="请选择提单！"
                    v-model='formData.id'>
                    <a-select-option v-for='(item,index) in billList' :key='index' :value='item.id'>
                      {{item.billnum }}  发往  {{item.sendSite}}
                    </a-select-option>
                  </a-select>
                </a-form-model-item>
              </a-col>
            </a-row>
            <!--  运单号 -->
            <a-row >
              <a-col   >
                <a-form-model-item label="出货号" >
                  <div v-for='(item,index) in deliveryNo '>
                    {{item.deliveryOrderNo}}
                  </div>
                </a-form-model-item>
              </a-col>
            </a-row>
          </a-form-model>
        </a-modal>
      </div>

    </div>
    <zm-delivery-detail v-show='visibleDetail'
                        :visibleDetail.sync="visibleDetail"
                        :deliveryData='deliveryDetail'
                        @edit='editDelvery'
    ></zm-delivery-detail>
    <zm-delivery-order-modal ref="modalForm" @ok="modalFormOk"></zm-delivery-order-modal>
  </a-card>
</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import ZmDeliveryOrderModal from './modules/ZmDeliveryOrderModal'
  import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import ZmDeliveryDetail from './detail'
  import { handleDetailss, putAction } from '../../../api/manage'

  export default {
    name: 'ZmDeliveryOrderList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      ZmDeliveryDetail,
      ZmDeliveryOrderModal
    },
    data () {
      return {
        description: '出货单管理页面',
        visibleDetail : false,
        deliveryDetail: {  },
        deliveryList:[],
        billList:[],
        sortNum:[],

        //加入提单弹窗的可见性
        visibleAddtoBill: false,
        //addtoBill
        formData:{
          ids:"",
          id:"",
        },
        deliveryNo:"",
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'出货单号',
            align:"center",
            dataIndex: 'deliveryOrderNo',
            scopedSlots: { customRender: 'httpurl' },
            customCell:this.cellClick
          },
          {
            title:'发往站点',
            align:"center",
            dataIndex: 'destination_dictText'
          },
          {
            title:'主品名',
            align:"center",
            dataIndex: 'mainName'
          },
          {
            title:'供应商',
            align:"center",
            dataIndex: 'supplier_dictText'
          },
          {
            title:'出货时间',
            align:"center",
            dataIndex: 'pickingTime'
          },
          {
            title:'发出站点',
            align:"center",
            dataIndex: 'departure_dictText'
          },
          {
            title:'参考号',
            align:"center",
            dataIndex: 'referenceNo'
          },
          {
            title:'备注',
            align:"center",
            dataIndex: 'remark'
          },
          {
            title:'属性',
            align:"center",
            dataIndex: 'attributes_dictText'
          },
          {
            title:'箱数',
            align:"center",
            dataIndex: 'caseNum'
          },
          {
            title:'重量',
            align:"center",
            dataIndex: 'weight'
          },
          {
            title:'价值',
            align:"center",
            dataIndex: 'value'
          },
          {
            title:'体积/泡比',
            align:"center",
            dataIndex: 'volumeBubbleRatio'
          },
          {
            title:'体积比',
            align:"center",
            dataIndex: 'volumeRatio'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/zmexpress/zmDeliveryOrder/list",
          delete: "/zmexpress/zmDeliveryOrder/delete",
          deleteBatch: "/zmexpress/zmDeliveryOrder/deleteBatch",
          exportXlsUrl: "/zmexpress/zmDeliveryOrder/exportXls",
          importExcelUrl: "zmexpress/zmDeliveryOrder/importExcel",
          
        },
        dictOptions:{},
        superFieldList:[],
      }
    },
    created() {
    this.getSuperFieldList();
    this.getBillDetails();
    this.getSortNum();
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      },
    },
    methods: {
      initDictConfig(){
      },
      /** 分类统计*/
      async getSortNum(){
        var list = '/zmexpress/zmDeliveryOrder/statisticsByStatus'
        this.sortNum =await handleDetailss(list, "").then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 子组件修改数据*/
      editDelvery(record) {
        this.handleEdit(record);

      },
      /** 根据状态展示*/
      async callback(key) {
        var list = '/zmexpress/zmDeliveryOrder/queryByStatusChange'
        this.deliveryList = await handleDetailss(list, { status :key }).then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 添加到提单*/
      submitAddtoBill: function() {
        var deliveryId = "";
        for (var a = 0; a < this.selectedRowKeys.length; a++) {
          deliveryId += this.selectedRowKeys[a] + ",";
        }
        var that = this;
        this.formData.ids = deliveryId;
        putAction('/zmexpress/zmDeliveryOrder/inToLading',that.formData).then((res) => {
          if (res.success) {
            that.reCalculatePage(that.selectedRowKeys.length)
            that.$message.success(res.message);
            that.visibleAddtoBill = false;
            that.loadData();
            that.onClearSelected();
          } else {
            that.$message.warning(res.message);
          }
        }).finally(() => {
          that.loading = false;
        });

      },
      /** 显示提单弹窗*/
      showAddToBill(){
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
        }
        this.visibleAddtoBill = true;
        this.getBillDetails();
        this.getDeliveryList(ids);
      },
      /** 获取所有可用提单*/
      async getBillDetails(){
        var list = '/zmexpress/zmBillloading/queryList'
        this.billList =await handleDetailss(list, "").then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 获取所有出货单*/
      async getDeliveryList(ids){
        var list = '/zmexpress/zmDeliveryOrder/queryListById'
        this.deliveryNo = await handleDetailss(list, { ids:ids }).then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 单元格点击事件*/
      cellClick(record) {
        return {
          style: {
            // 'color': 'blue', //这里将名称变了下色
          },
          on: {
            click: () => { //点击事件，也可以加其他事件
              this.deliveryDetail=record;
              this.visibleDetail=true;
            }
          }
        }
      },
      /**出货*/
      transfer:function() {
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
          var that = this;
          this.$confirm({
            title: "状态调整",
            content: "是否将该出货单状态调整为已出站？",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmDeliveryOrder/transfer',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'deliveryOrderNo',text:'出货单号',dictCode:''})
        fieldList.push({type:'sel_search',value:'destination',text:'发往站点',dictTable:'zm_airport', dictText:'name', dictCode:'name'})
        fieldList.push({type:'string',value:'mainName',text:'主品名',dictCode:''})
        fieldList.push({type:'string',value:'supplier',text:'供应商',dictCode:'zm_supplier,company,company'})
        fieldList.push({type:'datetime',value:'pickingTime',text:'出货时间'})
        fieldList.push({type:'sel_search',value:'departure',text:'发出站点',dictTable:'zm_airport', dictText:'name', dictCode:'name'})
        fieldList.push({type:'string',value:'referenceNo',text:'参考号',dictCode:''})
        fieldList.push({type:'string',value:'remark',text:'备注',dictCode:''})
        fieldList.push({type:'string',value:'attributes',text:'属性',dictCode:'delivery_order_item'})
        fieldList.push({type:'string',value:'caseNum',text:'箱数',dictCode:''})
        fieldList.push({type:'string',value:'weight',text:'重量',dictCode:''})
        fieldList.push({type:'string',value:'value',text:'价值',dictCode:''})
        fieldList.push({type:'string',value:'volumeBubbleRatio',text:'体积/泡比',dictCode:''})
        fieldList.push({type:'string',value:'volumeRatio',text:'体积比',dictCode:''})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>