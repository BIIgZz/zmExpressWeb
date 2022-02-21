<template>
  <a-card :body-style="{padding: '24px 32px'}" :bordered="false">
    <a-drawer
      title="运单"
      placement="right"
      width='1080'
      :closable="false"
      :visible="visibleBoxesForm"
      :after-visible-change="afterVisibleChange"
      @close="onClose"
    >
      <a-form @submit="handleSubmit" :form="form">
        <a-row >
          <a-col :span="12">
            <a-form-item
              label="箱号"
              :labelCol="{lg: {span: 7}, sm: {span: 7}}"
              :wrapperCol="{lg: {span: 15}, sm: {span: 17} }">
              <a-textarea
                rows="30"
                placeholder="请按一行一行的方式输入箱号"
                v-decorator="[
            'caseId',

          ]" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item
              label="运单号"
              :labelCol="{lg: {span: 7}, sm: {span: 7}}"
              :wrapperCol="{lg: {span: 15}, sm: {span: 17} }">
              <a-textarea
                rows="30"
                cols="10"
                placeholder="请按一行一行的方式输入运单号"
                v-decorator="[
            'wayBill',
          ]" />
            </a-form-item>
          </a-col>
        </a-row>
        <a-form-item
          :wrapperCol="{ span: 24 }"
          style="text-align: center"
        >
          <a-button htmlType="submit" type="primary">提交</a-button>
        </a-form-item>
      </a-form>
    </a-drawer>

  </a-card>
</template>

<script>
import { putAction } from '../../../api/manage'

export default {
  name: 'boxes-form',
  props:{
    visibleBoxesForm:{
      type:Boolean
    },
    deliveryData:{
      type:Object,
    },
  },
  data () {
    return {
      description: '将货箱放入提货单',
      value: 1,
      zmDeliveryIn:{
        ids:[],
        id:"",
        type:"",
      },
      // form
      form: this.$form.createForm(this),

    }
  },
  methods: {

    // handler
    handleSubmit (e) {
      e.preventDefault()
      this.form.validateFields((err, values) => {
        if (!err) {
          // eslint-disable-next-line no-console
          if (values.wayBill!=null){
            let waybillIds = values.wayBill.split(/[(\r\n)\r\n]+/);
            this.zmDeliveryIn.ids=waybillIds;
            this.zmDeliveryIn.id = this.deliveryData.id;
            this.zmDeliveryIn.type = 2;
            this.putData();
          }
          if (values.caseId!=null){
            let caseIds = values.caseId.split(/[(\r\n)\r\n]+/);
            this.zmDeliveryIn.ids=caseIds;
            this.zmDeliveryIn.id = this.deliveryData.id;
            this.zmDeliveryIn.type = 1;
            this.putData();
          }
        }
      })
    },
    putData:function(){
      let that  = this;
      putAction('/zmexpress/zmDeliveryOrder/deliveryByIds',this.zmDeliveryIn).then((res) => {
        if (res.success) {
          that.$emit('getNewStatistics')
          that.$message.success(res.message);
          that.form.resetFields();
        } else {
          that.$message.warning(res.message);
        }
      }).finally(() => {
        that.loading = false;
      });
    },
    submitMsg:function() {
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
          title: "确认修改",
          content: "是否修改选中数据?",
          onOk: function() {
            that.loading = true;
            putAction(  '/zmexpress/zmWaybill/cancel',{ ids: ids }).then((res) => {
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
    onClose() {
      // this.visible = false;
      this.$emit('update:visibleBoxesForm',false)
    },
    afterVisibleChange(val) {
    },
  }
}
</script>

<style scoped>

</style>