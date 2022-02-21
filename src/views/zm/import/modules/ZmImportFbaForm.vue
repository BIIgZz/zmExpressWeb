<template>
   <a-spin :spinning="confirmLoading">
     <j-form-container :disabled="formDisabled">
       <!-- 主表单区域 -->
       <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
         <a-row>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="FBA ID" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="fbaid">
              <a-input v-model="model.fbaid" placeholder="请输入FBA ID" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="客户订单号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="orderid">
              <a-input v-model="model.orderid" placeholder="请输入客户订单号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="服务" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="serviceId">
              <j-dict-select-tag type="list" v-model="model.serviceId"  dictCode="zm_service,name,name" placeholder="请选择服务" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="地址库编码" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="code">
              <j-search-select-tag v-model="model.code" dict="zm_fba_warehouse_detail,code,code" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="收件人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="name">
              <a-input v-model="model.name" placeholder="请输入收件人" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="公司" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="company">
              <a-input v-model="model.company" placeholder="请输入公司" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="address">
              <a-input v-model="model.address" placeholder="请输入地址" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="城市" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="city">
              <a-input v-model="model.city" placeholder="请输入城市" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="州" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="province">
              <a-input v-model="model.province" placeholder="请输入州" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="邮编" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="postcode">
              <a-input v-model="model.postcode" placeholder="请输入邮编" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="国家" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="countryCode">
              <j-dict-select-tag type="list" v-model="model.countryCode"  dictCode="zm_tool_countries,cname,cname" placeholder="请选择国家" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="电话" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="tel">
              <a-input v-model="model.tel" placeholder="请输入电话" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="邮箱" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="email">
              <a-input v-model="model.email" placeholder="请输入邮箱" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="总箱数" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="caseNumber">
              <a-input-number v-model="model.caseNumber" placeholder="请输入总箱数" style="width: 100%" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="物品属性" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="properties">
              <j-multi-select-tag type="checkbox" v-model="model.properties"  dictCode="item_properties" placeholder="请选择物品属性" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="报关方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsEclaration">
              <j-dict-select-tag type="radio" v-model="model.customsEclaration"  dictCode="customs_eclaration" placeholder="请选择报关方式" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="清关方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="customsClearance">
              <j-dict-select-tag type="radio" v-model="model.customsClearance"  dictCode="customs_clearance" placeholder="请选择清关方式" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="交税方式" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="taxPayment">
              <j-dict-select-tag type="radio" v-model="model.taxPayment"  dictCode="tax_payment" placeholder="请选择交税方式" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="交货条款" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="deliveryTerm">
              <j-dict-select-tag type="radio" v-model="model.deliveryTerm"  dictCode="terms_of_delivery" placeholder="请选择交货条款" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="VAT号" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="vat">
              <a-input v-model="model.vat" placeholder="请输入VAT号" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="参考号1" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="referenceNumber1">
              <a-input v-model="model.referenceNumber1" placeholder="请输入参考号1" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="参考号2" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="referenceNumber2">
              <a-input v-model="model.referenceNumber2" placeholder="请输入参考号2" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="note">
              <a-input v-model="model.note" placeholder="请输入备注" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="发件人姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="nameSender">
              <a-input v-model="model.nameSender" placeholder="请输入发件人姓名" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="公司" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="companySender">
              <a-input v-model="model.companySender" placeholder="请输入公司" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="addressSender">
              <a-input v-model="model.addressSender" placeholder="请输入地址" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="城市" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="citySender">
              <a-input v-model="model.citySender" placeholder="请输入城市" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="省份" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="provinceSender">
              <a-input v-model="model.provinceSender" placeholder="请输入省份" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="邮编" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="postcodeSender">
              <a-input v-model="model.postcodeSender" placeholder="请输入邮编" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="国家" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="countryCodeSender">
              <j-dict-select-tag type="list" v-model="model.countryCodeSender"  dictCode="zm_tool_countries,cname,cname" placeholder="请选择国家" />
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="电话" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="telSender">
              <a-input v-model="model.telSender" placeholder="请输入电话" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="邮箱" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="emailSender">
              <a-input v-model="model.emailSender" placeholder="请输入邮箱" ></a-input>
            </a-form-model-item>
          </a-col>
          <a-col :xs="24" :sm="12">
            <a-form-model-item label="客户" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="clientid">
              <a-input v-model="model.clientid" placeholder="请输入客户" ></a-input>
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
            :loading="zmImportGoodTable.loading"
            :columns="zmImportGoodTable.columns"
            :dataSource="zmImportGoodTable.dataSource"
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

  export default {
    name: 'ZmImportFbaForm',
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
            properties:"0",
            customsEclaration:0,
            customsClearance:0,
            taxPayment:0,
        },
        validatorRules: {
           fbaid: [
              { required: true, message: '请输入FBA ID!'},
           ],
           orderid: [
              { required: true, message: '请输入客户订单号!'},
           ],
           serviceId: [
              { required: true, message: '请输入服务!'},
           ],
           code: [
              { required: true, message: '请输入地址库编码!'},
           ],
           name: [
              { required: true, message: '请输入收件人!'},
           ],
           address: [
              { required: true, message: '请输入地址!'},
           ],
           city: [
              { required: true, message: '请输入城市!'},
           ],
           province: [
              { required: true, message: '请输入州!'},
           ],
           postcode: [
              { required: true, message: '请输入邮编!'},
           ],
           countryCode: [
              { required: true, message: '请输入国家!'},
           ],
        },
        // 新增时子表默认添加几行空数据
        addDefaultRowNum: 1,
        refKeys: ['zmImportGood', ],
        tableKeys:['zmImportGood', ],
        activeKey: 'zmImportGood',
        // 货柜详情
        zmImportGoodTable: {
          loading: false,
          dataSource: [],
          columns: [
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
              title: '货箱长度',
              key: 'length',
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
              title: '货箱高度',
              key: 'height',
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
              title: '申报数量',
              key: 'declaredNumber',
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
              type: FormTypes.select,
              dictCode:"type",
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:"0",
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
              type: FormTypes.image,
              token:true,
              responseName:"message",
              width:"200px",
              placeholder: '请输入${title}',
              defaultValue:'',
            },
          ]
        },
        url: {
          add: "/zmexpress/zmImportFba/add",
          edit: "/zmexpress/zmImportFba/edit",
          zmImportGood: {
            list: '/zmexpress/zmImportFba/queryZmImportGoodByMainId'
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
    },
    methods: {
     addBefore(){
            this.zmImportGoodTable.dataSource=[]
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
          this.requestSubTableData(this.url.zmImportGood.list, params, this.zmImportGoodTable)
        }
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
          zmImportGoodList: allValues.tablesValue[0].values,
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