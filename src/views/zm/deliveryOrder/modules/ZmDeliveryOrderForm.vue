<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="12">
            <a-form-model-item label="发往站点" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="destination">
              <j-search-select-tag v-model="model.destination" dict="zm_airport,name,name"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="主品名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="mainName">
              <a-input v-model="model.mainName" placeholder="请输入主品名"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="船公司" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="supplier">
              <j-dict-select-tag type="list" v-model="model.supplier" dictCode="zm_supplier,company,company" placeholder="请选择供应商" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="出货时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="pickingTime">
              <j-date placeholder="请选择出货时间"  v-model="model.pickingTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="发出站点" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="departure">
              <j-search-select-tag v-model="model.departure" dict="zm_airport,name,name"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="参考号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="referenceNo">
              <a-input v-model="model.referenceNo" placeholder="请输入参考号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="remark">
              <a-textarea v-model="model.remark" rows="4" placeholder="请输入备注" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="属性" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="attributes">
              <j-multi-select-tag type="checkbox" v-model="model.attributes" dictCode="delivery_order_item" placeholder="请选择属性" />
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
  </a-spin>
</template>

<script>

  import { httpAction, getAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'

  export default {
    name: 'ZmDeliveryOrderForm',
    components: {
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    data () {
      return {
        model:{
         },
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
        },
        url: {
          add: "/zmexpress/zmDeliveryOrder/add",
          edit: "/zmexpress/zmDeliveryOrder/edit",
          queryById: "/zmexpress/zmDeliveryOrder/queryById"
        }
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
       //备份model原始值
      this.modelDefault = JSON.parse(JSON.stringify(this.model));
    },
    methods: {
      add () {
        this.edit(this.modelDefault);
      },
      edit (record) {
        this.model = Object.assign({}, record);
        this.visible = true;
      },
      submitForm () {
        const that = this;
        // 触发表单验证
        this.$refs.form.validate(valid => {
          if (valid) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            httpAction(httpurl,this.model,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
            })
          }
         
        })
      },
    }
  }
</script>