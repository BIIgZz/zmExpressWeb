
<template>
  <div>
    <a-drawer
      title="运单列表"
      placement="right"
      width='960'
      :closable="false"
      :visible="visible"
      :after-visible-change="afterVisibleChange"
      @close="onClose"
    >
      <div style="margin-bottom: 16px">
        <a-button type="primary" :disabled="!hasSelected" :loading="loading" @click='moveOut'>
          移出提单
        </a-button>
        <span style="margin-left: 8px">
        <template v-if="hasSelected">
          {{ `选择了 ${selectedRowKeys.length} 项` }}
        </template>
      </span>
      </div>
      <a-table
        ref="table"
        size="middle"
        :scroll="{x:true}"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataList"
        :pagination="ipagination"
        :loading="loading"
        class="j-table-force-nowrap"
        :row-selection="{ selectedRowKeys: selectedRowKeys, onChange: onSelectChange }"
        >
        <a slot="name" slot-scope="text">{{ text }}</a>
      </a-table>
    </a-drawer>
  </div>
</template>

<script>
import { getAction } from '@api/manage'
import { JeecgListMixin } from '@/mixins/JeecgListMixin'
import { handleDetailss, putAction } from '../../../../api/manage'
import '@/assets/less/TableExpand.less'

export default {
  name: 'zm-bill-waybill-detail',
  mixins: [JeecgListMixin],
  props: {
    billDtail: {
      type: Object,
      default: null,
    }
  },
  data() {
    return {
      visible: false,
      data:[],
      columns : [

        {
          title: '用户',
          dataIndex: 'username',
          key: 'username',
          width: 80,
          sorter:true,
        },
        {
          title: '运单号',
          dataIndex: 'waybillId',
          key: 'waybillId',
          scopedSlots: { customRender: 'name' },
          customCell:this.cellClick,
          sorter:true,
        },
        {
          title: '服务',
          dataIndex: 'service',
          key: 'service',
          sorter:true,
        },
        {
          title: '件数',
          dataIndex: 'totalCost',
          key: 'totalCost',
          sorter:true,
        },
        {
          title: '重量',
          dataIndex: 'weightActual',
          key: 'weightActual',
          sorter:true,
        },
        {
          title: '体积',
          dataIndex: 'volume',
          key: 'volume',
          sorter:true,
        },
        {
          title: '发往国家',
          dataIndex: 'recipientCountry',
          key: 'recipientCountry',
          ellipsis: true,
          sorter:true,
        },
        {
          title: '承运',
          dataIndex: 'supplier',
          key: 'supplier',
          ellipsis: true,
        },
        {
          title: '拣货时间',
          dataIndex: 'updateTime',
          key: 'updateTime',
          sorter:true,
        },
      ],
      dataList:[],
      loading: false,
      selectedRowKeys: [],
    };
  },
  methods: {
    afterVisibleChange(val) {
    },
    showDrawer() {
      this.getWaybillList();
      this.visible = true;
    },
    onClose() {
      this.visible = false;
    },
    onSelectChange(selectedRowKeys) {
      this.selectedRowKeys = selectedRowKeys;
    },
    /** 单元格点击事件*/
    cellClick(record) {
      return {
        style: {
          // 'color': 'blue', //这里将名称变了下色
        },
        on: {
          click: () => { //点击事件，也可以加其他事件
            this.$router.push({path: '/src/views/zm/waybills/ZmWaybillsList',query:{id: record.waybillId}})
          }
        }
      }
    },
    /** 移除货箱*/
    moveOut: function() {
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
          title: "确认移出？",
          content: "是否移出选中运单?",
          onOk: function() {
            that.loading = true;
            putAction(  '/zmexpress/zmBillloading/moveOutWaybill', {ids: ids }).then((res) => {
              if (res.success) {
                //重新计算分页问题
                that.$message.success(res.message);
                that.getWaybillList();
                that.$emit("getNewStatistics");
                that.selectedRowKeys=[]
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
    async getWaybillList(){
      let params = this.billDtail.id
      this.dataList =await handleDetailss('/zmexpress/zmBillloading/queryWaybillList', {billnum: params}).then(res => {
        if (res.success) {
          return res.result;
        }
      })
    },
  },
  computed: {
    hasSelected() {
      return this.selectedRowKeys.length > 0;
    },
  },
};
</script>

<style scoped>

</style>
