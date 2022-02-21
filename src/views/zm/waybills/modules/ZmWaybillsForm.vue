<template>
   <a-spin :spinning="confirmLoading">
     <div >
       <a-row >
         <a-col  :span="8">
           <a-form-model-item label="客户名称"  :labelCol="labelCol" :wrapperCol="wrapperCol"  prop="name">
             <j-dict-select-tag type="list"  style="width:280px;"  v-model="model.username" dictCode="zm_client_main,username,username" placeholder="请选择客户名称" />
           </a-form-model-item>
         </a-col>
         <a-button type="primary"  :labelCol="labelCol" :wrapperCol="wrapperCol" icon="search" v-show='model.username==null'>下一步</a-button>
       </a-row>

     </div>
     <j-form-container :disabled="formDisabled" v-show='model.username!=null'>
       <!-- 主表单区域 -->
       <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
         <a-row>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="运单号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="waybillId">
              <a-input v-model="model.waybillId" placeholder="请输入运单号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="客户订单号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="orderId">
              <a-input v-model="model.orderId" placeholder="请输入客户订单号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="柜号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="caseId">
              <a-input v-model="model.caseId" placeholder="请输入柜号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="参考号一" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="referenceNumberOne">
              <a-input v-model="model.referenceNumberOne" placeholder="请输入参考号一" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="参考号二" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="referenceNumberTwo">
              <a-input v-model="model.referenceNumberTwo" placeholder="请输入参考号二" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="服务" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="service">
              <j-dict-select-tag type="list" v-model="model.service"  dictCode="zm_service,name,name" placeholder="请选择服务" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="供应商" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="supplier">
              <a-input v-model="model.supplier" placeholder="请输入供应商" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="供应商服务" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="supplierService">
              <a-input v-model="model.supplierService" placeholder="请输入供应商服务" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="店铺" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="shop">
              <a-input v-model="model.shop" placeholder="请输入店铺" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="Amazon Reference ID" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="amazonReferenceId">
              <a-input v-model="model.amazonReferenceId" placeholder="请输入Amazon Reference ID" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipient">
              <a-input v-model="model.recipient" placeholder="请输入收件人" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="公司名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="company">
              <a-input v-model="model.company" placeholder="请输入公司名" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="仓库代码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="warehouseId">
              <j-dict-select-tag type="list" v-model="model.warehouseId"  dictCode="zm_fba_warehouse_detail,code,code" placeholder="请选择仓库代码" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人地址一" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientAddressOne">
              <a-input v-model="model.recipientAddressOne" placeholder="请输入收件人地址一" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人地址二" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientAddressTwo">
              <a-input v-model="model.recipientAddressTwo" placeholder="请输入收件人地址二" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人地址三" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientAddressTri">
              <a-input v-model="model.recipientAddressTri" placeholder="请输入收件人地址三" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人城市" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientCity">
              <a-input v-model="model.recipientCity" placeholder="请输入收件人城市" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人省/州" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientState">
              <a-input v-model="model.recipientState" placeholder="请输入收件人省/州" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人邮编" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientZipOde">
              <a-input v-model="model.recipientZipOde" placeholder="请输入收件人邮编" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人国家" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientCountry">
              <a-input v-model="model.recipientCountry" placeholder="请输入收件人国家" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人电话" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientTel">
              <a-input v-model="model.recipientTel" placeholder="请输入收件人电话" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人邮箱" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="recipientEmail">
              <a-input v-model="model.recipientEmail" placeholder="请输入收件人邮箱" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="主品名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="mainProductName">
              <a-input v-model="model.mainProductName" placeholder="请输入主品名" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="报关方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsDeclaration">
              <j-dict-select-tag type="radio" v-model="model.customsDeclaration"  dictCode="customs_eclaration" placeholder="请选择报关方式" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="清关方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsClearance">
              <j-dict-select-tag type="radio" v-model="model.customsClearance"  dictCode="customs_clearance" placeholder="请选择清关方式" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="交货条款" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="termsDelivery">
              <j-dict-select-tag type="radio" v-model="model.termsDelivery"  dictCode="terms_of_delivery" placeholder="请选择交货条款" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="交税方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="taxPayment">
              <j-dict-select-tag type="radio" v-model="model.taxPayment"  dictCode="tax_payment" placeholder="请选择交税方式" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="VAT号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="vat">
              <a-input v-model="model.vat" placeholder="请输入VAT号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="属性" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="attributes">
              <j-multi-select-tag type="checkbox" v-model="model.attributes"  dictCode="item_properties" placeholder="请选择属性" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="问题件" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="problem">
              <a-input v-model="model.problem" placeholder="请输入问题件" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="客服代表" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deputyCustomer">
              <a-input v-model="model.deputyCustomer" placeholder="请输入客服代表" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="销售代表" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deputySale">
              <a-input v-model="model.deputySale" placeholder="请输入销售代表" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="财务代表" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deputyFinancial">
              <a-input v-model="model.deputyFinancial" placeholder="请输入财务代表" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="自定义标识" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customLogo">
              <a-input v-model="model.customLogo" placeholder="请输入自定义标识" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="物品属性" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="itemAttributes">
              <a-input v-model="model.itemAttributes" placeholder="请输入物品属性" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="内部备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="inRemark">
              <a-input v-model="model.inRemark" placeholder="请输入内部备注" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="remark">
              <a-input v-model="model.remark" placeholder="请输入备注" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="承运商" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="carrier">
              <a-input v-model="model.carrier" placeholder="请输入承运商" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="跟踪号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="trackingNumber">
              <a-input v-model="model.trackingNumber" placeholder="请输入跟踪号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="计费时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="timeBillable">
              <j-date placeholder="请选择计费时间" v-model="model.timeBillable" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="签收时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="timeSubmission">
              <j-date placeholder="请选择签收时间" v-model="model.timeSubmission" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="拣货时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="timePicking">
              <j-date placeholder="请选择拣货时间" v-model="model.timePicking" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="出货时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="timeDelivery">
              <j-date placeholder="请选择出货时间" v-model="model.timeDelivery" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="下单时间" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="timeOrder">
              <j-date placeholder="请选择下单时间" v-model="model.timeOrder" style="width: 100%" />
            </a-form-model-item>
          </a-col>
        </a-row>
      </a-form-model>
     </j-form-container>
      <!-- 子表单区域 -->
      <a-tabs v-model="activeKey" @change="handleChangeTabs">
        <a-tab-pane tab="货柜详情" :key="refKeys[0]" :forceRender="true">
          <j-editable-table
            :ref="refKeys[0]"
            :loading="zmGoodCaseTable.loading"
            :columns="zmGoodCaseTable.columns"
            :dataSource="zmGoodCaseTable.dataSource"
            :maxHeight="300"
            :disabled="formDisabled"
            :rowNumber="true"
            :rowSelection="true"
            :actionButton="true"/>
        </a-tab-pane>
      </a-tabs>
    </a-spin>
</template>

<script>

  import { FormTypes,getRefPromise,VALIDATE_NO_PASSED } from '@/utils/JEditableTableUtil'
  import { JEditableTableModelMixin } from '@/mixins/JEditableTableModelMixin'
  import { validateDuplicateValue } from '@/utils/util'
  const ruleBaseURL = '/sys/fillRule/executeRuleByCode/'
  import { putAction } from '@api/manage'
  export default {
    name: 'ZmWaybillsForm',
    mixins: [JEditableTableModelMixin],
    components: {
    },
    data() {
      return {
        labelCol: {
          xs: { span: 24 },
          sm: { span: 6 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        labelCol2: {
          xs: { span: 24 },
          sm: { span: 3 },
        },
        wrapperCol2: {
          xs: { span: 24 },
          sm: { span: 20 },
        },
        model:{
        },
        validatorRules: {
        },
        // 新增时子表默认添加几行空数据
        addDefaultRowNum: 1,
        refKeys: ['zmGoodCase', ],
        tableKeys:['zmGoodCase', ],
        activeKey: 'zmGoodCase',
        // 货柜详情
        zmGoodCaseTable: {
          loading: false,
          dataSource: [],
          columns: [
            {
              title: 'fbaid',
              key: 'fbaid',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '货箱编号',
              key: 'caseid',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '货箱重量',
              key: 'weight',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '实际重量',
              key: 'weightActually',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '实际长度',
              key: 'lengthActually',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '实际重量',
              key: 'heightActually',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '货箱长度',
              key: 'length',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '货箱高度',
              key: 'height',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '货箱宽度',
              key: 'width',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '实际宽度',
              key: 'widthActually',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '中文名称',
              key: 'cnName',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '英文名称',
              key: 'enName',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '申报单价',
              key: 'declaredPrice',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '产品海关编码',
              key: 'hscode',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '产品材料',
              key: 'material',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '产品用途',
              key: 'application',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '品牌',
              key: 'brand',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '品牌类型',
              key: 'type',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '产品型号',
              key: 'model',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '销售链接',
              key: 'link',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '产品售价',
              key: 'price',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '图片链接',
              key: 'picture',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
            {
              title: '申报数量',
              key: 'declaredNumber',
              type: FormTypes.input,
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },

          ]
        },
        url: {
          add: "/zmexpress/zmWaybills/add",
          edit: "/zmexpress/zmWaybills/edit",
          zmGoodCase: {
            list: '/zmexpress/zmWaybills/queryZmGoodCaseByMainId'
          },
          rule: {
            orderNo: ruleBaseURL + 'shop_order_num'
          },
        }
      }
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
      this.getOrderNum();
    },
    methods: {
     addBefore(){
            this.zmGoodCaseTable.dataSource=[]
      },
      getAllTable() {
        let values = this.tableKeys.map(key => getRefPromise(this, key))
        return Promise.all(values)
      },
      /** 调用完edit()方法之后会自动调用此方法 */
      editAfter() {
        this.$nextTick(() => {
        })
        // 加载子表数据
        if (this.model.id) {
          let params = { id: this.model.id }
          this.requestSubTableData(this.url.zmGoodCase.list, params, this.zmGoodCaseTable)
        }
      },
      /** 打开抽屉 */
      showDrawer() {
        this.getOrderNum();
      },
      /** 获取订单号*/
      getOrderNum() {
        putAction(this.url.rule.orderNo, this.model).then(res => {
          // 执行成功，获取返回的值，并赋到页面上
          if (res.success) {
            console.log(res);
            if( this.model.orderId == null)
              this.model.orderId = res.result
          }
        })
      },
      //校验所有一对一子表表单
    validateSubForm(allValues){
        return new Promise((resolve,reject)=>{
          Promise.all([
          ]).then(() => {
            resolve(allValues)
          }).catch(e => {
            if (e.error === VALIDATE_NO_PASSED) {
              // 如果有未通过表单验证的子表，就自动跳转到它所在的tab
              this.activeKey = e.index == null ? this.activeKey : this.refKeys[e.index]
            } else {
              console.error(e)
            }
          })
        })
    },
      /** 整理成formData */
      classifyIntoFormData(allValues) {
        let main = Object.assign(this.model, allValues.formValue)

        return {
          ...main, // 展开
          zmGoodCaseList: allValues.tablesValue[0].values,
        }
      },
      validateError(msg){
        this.$message.error(msg)
      },
     close() {
        this.visible = false
        this.$emit('close')
        this.$refs.form.clearValidate();
      },

    }
  }
</script>

<style scoped>
</style>