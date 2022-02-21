<template>
  <a-drawer
    title="货箱详情"
    placement="right"
    :closable="false"
    width='1080'
    :visible="visibleCaseList"
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
    <a-table :columns="columns" :data-source="caseList" size='small'
             :row-selection="{ selectedRowKeys: selectedRowKeys, onChange: onSelectChange }"
             rowKey='id'>
      <a slot="name" slot-scope="text">{{ text }}</a>
    </a-table>
  </a-drawer>

</template>

<script>
import { handleDetailss, putAction } from '../../../api/manage'

export default {
  name: 'deliveryAndCaseList',
  props: {
    visibleCaseList: {
      type: Boolean
    },
    deliveryData: {
      type: Object,
    },
    newStatistics:{
      type:Object,
    }
  },
  watch: {
    deliveryData: {
      handler(newVal, oldVal) {
        this.getCaseList(newVal.id);
      },
      immediate: true
    },
    newStatistics:{
      handler(newVal, oldVal) {
        this.getCaseList(newVal.id);
      },
      immediate: true
    }
  },
  data() {
    return {

      columns:[
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
      caseList: [],
      loading: false,
      selectedRowKeys: [],
    };
  },
  methods: {
    afterVisibleChange(val) {
    },
    showDrawer(record) {
      // this.visible = true;
      this.$router.push({ path: '/src/views/zm/waybills/ZmWaybillsList', query: { id: record.waybillId } })
    },
    //获取运单列表
    async getCaseList() {
      var list = '/zmexpress/zmDeliveryOrder/getCaseListByDeliveryId'
      var that = this;
      this.caseList = await handleDetailss(list, { id: this.deliveryData.id }).then(res => {
        if (res.success) {
          return res.result;
        }
      });
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
            putAction(  '/zmexpress/zmDeliveryOrder/moveOutDelivery', {ids: ids }).then((res) => {
              if (res.success) {
                //重新计算分页问题
                that.$message.success(res.message);
                that.getCaseList();
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
            this.$router.push({ path: '/src/views/zm/waybills/ZmWaybillsList', query: { id: record.waybillId } })
          }
        }
      }
    },
    onSelectChange(selectedRowKeys) {
      this.selectedRowKeys = selectedRowKeys;
    },
    onClose() {
      // this.visible = false;
      this.$emit('update:visibleCaseList', false)
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