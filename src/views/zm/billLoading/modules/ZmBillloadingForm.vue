<template>
  <a-spin :spinning="confirmLoading">
    <j-form-container :disabled="formDisabled">
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
          <a-col :span="12">
            <a-form-model-item label="柜号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="billnum">
              <a-input v-model="model.billnum" placeholder="请输入柜号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="提单号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="inBillnum">
              <a-input v-model="model.inBillnum" placeholder="请输入提单号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="类型" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="type">
              <j-dict-select-tag type="list" v-model="model.type" dictCode="bill_loding" placeholder="请选择类型" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="供应商" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="supplier">
              <j-search-select-tag v-model="model.supplier" dict="zm_supplier  ,company,company"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="船公司" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="arriage">
              <j-search-select-tag v-model="model.arriage" dict="zm_shipping_company,name,abbreviation"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="航次" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="shift">
              <a-input v-model="model.shift" placeholder="请输入班次"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="船名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="shipNumber">
              <a-input v-model="model.shipNumber" placeholder="请输入船号"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="集装箱规格" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="container">
              <j-dict-select-tag type="list" v-model="model.container" dictCode="zm_container_standards,standard,standard" placeholder="请选择集装箱规格" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="清关公司" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsClearance">
              <j-search-select-tag v-model="model.customsClearance" dict="zm_supplier where type like '%7%' ,company,company"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="报关公司" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="declarationCompany">
              <j-search-select-tag v-model="model.declarationCompany" dict="zm_supplier where type like '%6%' ,company,company"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="出发站点" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="departurePoint">
              <j-search-select-tag v-model="model.departurePoint" dict="zm_airport,name,name"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="发往站点" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="sendSite">
              <j-search-select-tag v-model="model.sendSite" dict="zm_airport,name,name"  />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="工作号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="workNo">
              <a-input v-model="model.workNo" placeholder="请输入工作号" style="width: 100%" />
            </a-form-model-item>
          </a-col>
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="合箱数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="allBoxes">-->
<!--              <a-input-number v-model="model.allBoxes" placeholder="请输入合箱数" style="width: 100%" />-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="价值" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="worth">-->
<!--              <a-input v-model="model.worth" placeholder="请输入价值"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="实重" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="weight">-->
<!--              <a-input v-model="model.weight" placeholder="请输入实重"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="体积比" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="volumeRatio">-->
<!--              <a-input v-model="model.volumeRatio" placeholder="请输入体积比"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="应收" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="receivable">-->
<!--              <a-input v-model="model.receivable" placeholder="请输入应收"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="提单应付" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="billLadingPayable">-->
<!--              <a-input v-model="model.billLadingPayable" placeholder="请输入提单应付"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="运单应付" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="waybillPayable">-->
<!--              <a-input v-model="model.waybillPayable" placeholder="请输入运单应付"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="体积" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="volume">-->
<!--              <a-input v-model="model.volume" placeholder="请输入体积"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="12">-->
<!--            <a-form-model-item label="盈亏" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="profit">-->
<!--              <a-input v-model="model.profit" placeholder="请输入盈亏"  ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
          <a-col :span="12">
            <a-form-model-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="remark">
              <a-input v-model="model.remark" placeholder="请输入备注"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="内部备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="inRemark">
              <a-input v-model="model.inRemark" placeholder="请输入内部备注"  ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="出发时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="departure">
              <j-date placeholder="请选择出发时间"  v-model="model.departure" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="到达时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="arrive">
              <j-date placeholder="请选择到达时间"  v-model="model.arrive" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :span="12">
            <a-form-model-item label="清关时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsClearanceTime">
              <j-date placeholder="请选择清关时间"  v-model="model.customsClearanceTime" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%" />
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
  name: 'ZmBillloadingForm',
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
        billnum: [
          { required: true, message: '请输入柜号!'},
        ],
        inBillnum: [
          { required: true, message: '请输入提单号!'},
        ],
        type: [
          { required: true, message: '请输入类型!'},
        ],
        arriage: [
          { required: true, message: '请输入船公司!'},
        ],

        departurePoint: [
          { required: true, message: '请输入发出站点!'},
        ],
        sendSite: [
          { required: true, message: '请输入发往站点!'},
        ],
      },
      url: {
        add: "/zmexpress/zmBillloading/add",
        edit: "/zmexpress/zmBillloading/edit",
        queryById: "/zmexpress/zmBillloading/queryById"
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