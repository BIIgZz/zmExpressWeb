<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('用户主表')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-tabs default-active-key="0"  @change='tabClick' type='card'>
        <a-tab-pane :label="item" :name="item" v-for="(item, index) in sortNum" :key="index">
          <template slot="tab">
            <p>{{item}}</p>
          </template>
        </a-tab-pane>
      </a-tabs>


      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        class="j-table-force-nowrap"
        :scroll="{x:true}"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange, type:'radio'}"
        :customRow="clickThenSelect"
        @change="handleTableChange">
        <a slot="detail" slot-scope="text">{{ text }}</a>
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
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>
    <a-drawer
      :title="title"
      placement="right"
      :closable="false"
      :width="1200"
      :visible="visible"
      :after-visible-change="afterVisibleChange"
      @close="onClose"
    >

      <a-tabs defaultActiveKey="1"  >
        <a-tab-pane tab="用户基础信息" key="1" >
          <ZmClientBasicList :mainId="selectedMainId" />
        </a-tab-pane>
        <a-tab-pane tab="用户地址" key="2" forceRender>
          <ZmClientAddressList :mainId="selectedMainId" />
        </a-tab-pane>
        <a-tab-pane tab="财务数据" key="3" forceRender>
          <ZmClientFinanceList :mainId="selectedMainId" />
        </a-tab-pane>
        <a-tab-pane tab="认证资料" key="4" forceRender>
          <ZmClientIdentity :mainId="userId" @sortNum="getSortNum"/>
        </a-tab-pane>

      </a-tabs>

    </a-drawer>


    <zmClientMain-modal ref="modalForm" @ok="modalFormOk"></zmClientMain-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import ZmClientMainModal from './modules/ZmClientMainModal'
  import { getAction } from '@/api/manage'
  import ZmClientBasicList from './ZmClientBasicList'
  import ZmClientAddressList from './ZmClientAddressList'
  import ZmClientFinanceList from './ZmClientFinanceList'
  import ZmClientIdentity from './ZmClientIdentity'
  import '@/assets/less/TableExpand.less'
  import { handleDetailss } from '../../../api/manage'

  export default {
    name: "ZmClientMainList",
    mixins:[JeecgListMixin],
    components: {
      ZmClientBasicList,
      ZmClientAddressList,
      ZmClientFinanceList,
      ZmClientMainModal,
      ZmClientIdentity,

    },
    data () {
      return {
        visible: false,
        description: '用户主表管理页面',
        disableMixinCreated:true,
        // 表头
        columns: [
          {
            title:'用户编码',
            align:"center",
            dataIndex: 'code',
            scopedSlots: { customRender: 'detail' },
            customCell:this.cellClick
          },
          {
            title:'用户名',
            align:"center",
            dataIndex: 'username'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' },
          }
        ],

        url: {
          list: "/zmexpress/zmClientMain/list",
          delete: "/zmexpress/zmClientMain/delete",
          deleteBatch: "/zmexpress/zmClientMain/deleteBatch",
          exportXlsUrl: "/zmexpress/zmClientMain/exportXls",
          importExcelUrl: "zmexpress/zmClientMain/importExcel",
        },
        dictOptions:{
        },
        /* 分页参数 */
        ipagination:{
          current: 1,
          pageSize: 5,
          pageSizeOptions: ['5', '10', '50'],
          showTotal: (total, range) => {
            return range[0] + "-" + range[1] + " 共" + total + "条"
          },
          showQuickJumper: true,
          showSizeChanger: true,
          total: 0
        },
        userId:'',
        selectedMainId:'',
        superFieldList:[],
        title:'',
        sortNum:[],
      }
    },
    created() {
      this.tabClick(0);
      this.getSortNum();
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    },
    methods: {
      initDictConfig(){
      },
      clickThenSelect(record) {
        return {
          on: {
            click: () => {
              this.onSelectChange(record.id.split(","), [record]);
              this.title=record.code+"/"+record.username;
              this.userId = record.code;
            }
          }
        }
      },
      /** 分类统计*/
      async getSortNum(){
        var list = '/zmexpress/zmClientMain/statisticsByStatus'
        var that = this;

        this.sortNum =await handleDetailss(list, "").then(res => {
          if (res.success) {
            that.reCalculatePage(that.selectedRowKeys.length)
            return res.result;
          }
        })
      },
      tabClick(val){

        if (val!=5)
         this.queryParam.status=val;
        else
          this.queryParam.status='';
        this.loadData(1);
      },
      cellClick(record) {
        return {
          style: {
            // 'color': 'blue', //这里将名称变了下色
          },
          on: {
            click: () => { //点击事件，也可以加其他事件
              this.showDrawer();
            }
          }
        }
      },
      afterVisibleChange(val) {
        console.log('visible', val);
      },
      showDrawer() {
        this.visible = true;
      },
      onClose() {
        this.visible = false;
      },
      onClearSelected() {
        this.selectedRowKeys = [];
        this.selectionRows = [];
        this.selectedMainId=''
      },
      onSelectChange(selectedRowKeys, selectionRows) {
        this.selectedMainId=selectedRowKeys[0]
        this.selectedRowKeys = selectedRowKeys;
        this.selectionRows = selectionRows;
      },
      loadData(arg) {
        if(!this.url.list){
          this.$message.error("请设置url.list属性!")
          return
        }
        //加载数据 若传入参数1则加载第一页的内容
        if (arg === 1) {
          this.ipagination.current = 1;
        }
        this.onClearSelected()
        var params = this.getQueryParams();//查询条件
        console.log("status",this.queryParam.status);
        console.log("param",params)
        this.loading = true;
        getAction(this.url.list, params).then((res) => {
          if (res.success) {
            this.dataSource = res.result.records;
            this.ipagination.total = res.result.total;
          }
          if(res.code===510){
            this.$message.warning(res.message)
          }
          this.loading = false;
        })
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'code',text:'用户编码',dictCode:''})
        fieldList.push({type:'string',value:'username',text:'用户名',dictCode:''})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>