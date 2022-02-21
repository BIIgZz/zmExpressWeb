<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="8">
            <a-form-model-item label="服务名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="name">
              <a-input v-model="model.name" placeholder="请输入服务名称"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="服务代码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="serviceCode">
              <a-input v-model="model.serviceCode" placeholder="请输入服务代码"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="服务分类" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="serviceSort">
              <j-dict-select-tag type="list" v-model="model.serviceSort" dictCode="service_sort" placeholder="请选择服务分类" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="计费方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="billingPlan">
              <j-dict-select-tag type="list" v-model="model.billingPlan" dictCode="bill" placeholder="请选择计费方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="计重方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="weighingMethod">
              <j-dict-select-tag type="list" v-model="model.weighingMethod" dictCode="weight" placeholder="请选择计重方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="进位方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="carryMethod">
              <j-dict-select-tag type="list" v-model="model.carryMethod" dictCode="carry_method" placeholder="请选择进位方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="计泡系数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="bubble">
              <a-input v-model="model.bubble" placeholder="请输入计泡系数"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="分泡比例" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="bubbleSplittingRatio">
              <a-input-number v-model="model.bubbleSplittingRatio" placeholder="请输入分泡比例" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="销售提成基数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="salesCommissionBase">
              <j-dict-select-tag type="list" v-model="model.salesCommissionBase" dictCode="sales_base" placeholder="请选择销售提成基数" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="销售提成比例" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="salesCommission">
              <a-input v-model="model.salesCommission" placeholder="请输入销售提成比例"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="销售提成单价" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="salesCommissionUnitPrice">
              <a-input v-model="model.salesCommissionUnitPrice" placeholder="请输入销售提成单价"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="运单号规则" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="numberRules">
              <a-input v-model="model.numberRules" placeholder="请输入运单号规则"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="标签代码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="tagCode">
              <a-input v-model="model.tagCode" placeholder="请输入标签代码"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="运单号规则" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="waybilRules">
              <a-input v-model="model.waybilRules" placeholder="请输入运单号规则"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="申报后补" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="supplementAfterDeclaration">
              <j-switch v-model="model.supplementAfterDeclaration"  ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="状态" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="status">
              <j-dict-select-tag type="list" v-model="model.status" dictCode="service_status" placeholder="请选择状态" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="取消方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="cancelMethod">
              <j-dict-select-tag type="list" v-model="model.cancelMethod" dictCode="cancel_method" placeholder="请选择取消方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="发件人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="sender">
              <j-dict-select-tag type="list" v-model="model.sender" dictCode="fill_in" placeholder="请选择发件人" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="支持报关方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsDeclarationMethod">
              <j-multi-select-tag type="checkbox" v-model="model.customsDeclarationMethod" dictCode="customs_eclaration" placeholder="请选择支持报关方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="支持清关方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsLearanceMethod">
              <j-multi-select-tag type="checkbox" v-model="model.customsLearanceMethod" dictCode="customs_clearance" placeholder="请选择支持清关方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="支持交税方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="taxPaymentMethod">
              <j-multi-select-tag type="checkbox" v-model="model.taxPaymentMethod" dictCode="tax_payment" placeholder="请选择支持交税方式" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="支持交货条款" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryTerms">
              <j-multi-select-tag type="checkbox" v-model="model.deliveryTerms" dictCode="delivery_terms" placeholder="请选择支持交货条款" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="票记重精度" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="ticketWeightAccuracy">
              <a-input v-model="model.ticketWeightAccuracy" placeholder="请输入票记重精度"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="箱记重精度" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="caseWeightAccuracy">
              <a-input v-model="model.caseWeightAccuracy" placeholder="请输入箱记重精度"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="尺寸精度" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="sizeAccuracy">
              <a-input v-model="model.sizeAccuracy" placeholder="请输入尺寸精度"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最小件数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minPieceNum">
              <a-input v-model="model.minPieceNum" placeholder="请输入最小件数"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最大件数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="maxPieceNum">
              <a-input v-model="model.maxPieceNum" placeholder="请输入最大件数"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最低箱实重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minBoxWeight">
              <a-input v-model="model.minBoxWeight" placeholder="请输入最低箱实重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最低箱材重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minBoxMaterialWeight">
              <a-input v-model="model.minBoxMaterialWeight" placeholder="请输入最低箱材重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最低箱收费重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minBoxChargeWeight">
              <a-input v-model="model.minBoxChargeWeight" placeholder="请输入最低箱收费重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最低箱均重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minBoxAverageWeight">
              <a-input v-model="model.minBoxAverageWeight" placeholder="请输入最低箱均重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最低票收费重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minTicketChargeWeight">
              <a-input v-model="model.minTicketChargeWeight" placeholder="请输入最低票收费重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最小票实重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="minTicketWeight">
              <a-input v-model="model.minTicketWeight" placeholder="请输入最小票实重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最大票实重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="maxTicketWeight">
              <a-input v-model="model.maxTicketWeight" placeholder="请输入最大票实重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最大票收费重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="maxTicketChargeWeight">
              <a-input v-model="model.maxTicketChargeWeight" placeholder="请输入最大票收费重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="最大箱实重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="maxBoxWeight">
              <a-input v-model="model.maxBoxWeight" placeholder="请输入最大箱实重"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="用户等级" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="userLevel">
              <j-dict-select-tag type="list" v-model="model.userLevel" dictCode="zm_user_level,name,name" placeholder="请选择用户等级" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="指定用户" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="designatedUser">
              <a-input v-model="model.designatedUser" placeholder="请输入指定用户"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="时效" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="aging">
              <a-input v-model="model.aging" placeholder="请输入时效"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="默认交货区域" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="area">
              <a-input v-model="model.area" placeholder="请输入默认交货区域"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="是否显示" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="display">
              <j-switch v-model="model.display"  ></j-switch>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="带电标识" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="electric">
              <a-input v-model="model.electric" placeholder="请输入带电标识"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="到达国家" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="country">
              <a-textarea v-model="model.country" rows="4" placeholder="请输入到达国家" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="申报币种" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="reportingCurrency">
              <j-multi-select-tag type="list_multi" v-model="model.reportingCurrency" dictCode="currency" placeholder="请选择申报币种" />
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="创建人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createBy">
              <a-input v-model="model.createBy" placeholder="请输入创建人"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="8">
            <a-form-model-item label="创建日期" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="createTime">
              <a-input v-model="model.createTime" placeholder="请输入创建日期"  ></a-input>
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
    name: 'ZmServiceForm',
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
           name: [
              { required: true, message: '请输入服务名称!'},
           ],
           serviceCode: [
              { required: true, message: '请输入服务代码!'},
           ],
        },
        url: {
          add: "/zmexpress/zmService/add",
          edit: "/zmexpress/zmService/edit",
          queryById: "/zmexpress/zmService/queryById"
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