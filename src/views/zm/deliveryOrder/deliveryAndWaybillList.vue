<template>
  <a-drawer
    title="运单"
    placement="right"
    width='1080'
    :closable="false"
    :visible="visibleWaybillList"
    :after-visible-change="afterVisibleChange"
    @close="onClose"
  >
    <div style="margin-bottom: 16px">
      <a-button type="primary" :disabled="!hasSelected" :loading="loading" @click='moveOut'>
        移出出货单
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
      :dataSource="wayBillList"
      class="j-table-force-nowrap"
      :row-selection="{ selectedRowKeys: selectedRowKeys, onChange: onSelectChange }"
    >
      <span slot="status" slot-scope="text"> <a-badge v-if="text.search('/')!=-1" status="success" />{{ text }} </span>
      <a slot="name" slot-scope="text">{{ text }}</a>
    </a-table>
<!--    <a-table :columns="columns" :data-source="wayBillList">-->
<!--      <a slot="name" slot-scope="text,record">{{ text }}</a>-->
<!--    </a-table>-->
  </a-drawer>
</template>

<script>
import { handleDetailss, putAction } from '../../../api/manage'



export default {
  name: 'delivery-and-waybillList',
  props:{
    visibleWaybillList:{
      type:Boolean
    },
    deliveryData:{
      type:Object,
    },
    newStatistics:{
      type:Object,
    }
  },
  watch: {
    deliveryData: {
      handler(newVal, oldVal) {
        this.getWayBillList(newVal.id);
      },
      immediate:true
    },
    newStatistics:{
      handler(newVal, oldVal) {
        this.getWayBillList(newVal.id);
      },
      immediate: true
    }
  },
  data() {
    return {
      loading: false,
      data:[],
      columns : [
        {
          title: '用户',
          dataIndex: 'username',
          key: 'username',

        },
        {
          title: '运单号',
          dataIndex: 'waybillId',
          key: 'waybillId',
          scopedSlots: { customRender: 'name' },
          customCell:this.cellClick
        },
        {
          title: '服务',
          dataIndex: 'service',
          key: 'service',
          ellipsis: true,
        },
        {
          title: '件数',
          dataIndex: 'pieces',
          key: 'pieces',
          ellipsis: true,
          scopedSlots: { customRender: 'status' },
        },
        {
          title: '体积',
          dataIndex: 'volume',
          key: 'volume',
          ellipsis: true,
        },
        {
          title: '重量',
          dataIndex: 'weightActual',
          key: 'weightActual',
          ellipsis: true,
        },
        {
          title: '发往国家',
          dataIndex: 'recipientCountry',
          key: 'recipientCountry',
          ellipsis: true,
        },
        {
          title: '承运',
          dataIndex: 'carrier',
          key: 'carrier',
          ellipsis: true,
        },
        // {
        //   title: '查货',
        //   dataIndex: 'address',
        //   key: 'address 7',
        //   ellipsis: true,
        // },
        {
          title: '拣货时间',
          dataIndex: 'createTime',
          key: 'createTime',
          ellipsis: true,
        },

      ],
      wayBillList:[],
      selectedRowKeys: [],
    };
  },
  methods: {
    afterVisibleChange(val) {
    },
    showDrawer(record) {
      // this.visible = true;
      this.$router.push({path: '/src/views/zm/waybills/ZmWaybillsList',query:{id: record.waybillId}})
    },
    //获取运单列表
    async getWayBillList(){
      var list = '/zmexpress/zmDeliveryOrder/getTotalWaybillByDelivery'
      var  that = this;
      this.wayBillList =await handleDetailss(list, { id: this.deliveryData.id}).then(res => {
        if (res.success) {
          return res.result;
        }
      });
      console.log(this.wayBillList)
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
          content: "是否移出选中货箱?",
          onOk: function() {
            that.loading = true;
            putAction(  '/zmexpress/zmDeliveryOrder/moveOutWaybill', {ids: ids }).then((res) => {
              if (res.success) {
                //重新计算分页问题
                that.$message.success(res.message);
                that.getWayBillList();
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
    onSelectChange(selectedRowKeys) {
      this.selectedRowKeys = selectedRowKeys;
    },
    onClose() {
      // this.visible = false;
      this.$emit('update:visibleWaybillList',false)
    },
  },
  computed: {
    hasSelected() {
      return this.selectedRowKeys.length > 0;
    },
  },
}
</script>

<style scoped>

</style>