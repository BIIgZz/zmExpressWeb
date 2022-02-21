<template>



  <a-card :bordered="false">
    <a-form-model ref="form" :model="model" :rules="rules" @submit="handleSubmit">
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-model-item label="柜号" prop="note" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-input v-model="model.note"/>
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-model-item label="信息" prop="gender" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-select v-model="model.gender" placeholder="Select a option and change input text above" @change="handleSelectChange">
              <a-select-option value="male">male</a-select-option>
              <a-select-option value="female">female</a-select-option>
            </a-select>
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-row>
        <a-col :md="24" :sm="24">
          <a-form-model-item label="Gender" prop="cascader" :labelCol="{ span: 7 }" :wrapperCol="{ span: 15 }">
            <a-cascader :options="areaOptions" @change="onChange" :showSearch="{filter}" placeholder="Please select" />
          </a-form-model-item>
        </a-col>
      </a-row>
      <a-form-model-item :wrapperCol="{ span: 12, offset: 5 }">
        <a-col :md="24" :sm="24">
          <a-form-model-item :wrapperCol="{ span: 12, offset: 5 }">
            <a-button type="primary" htmlType="submit">Submit</a-button>
          </a-form-model-item>
        </a-col>
      </a-form-model-item>
    </a-form-model>
  </a-card>
</template>

<script>
import { getAction } from '@/api/manage'
export default {
  name:"zm-logistics-add",
  props: ['sex','name'],
  data () {
    return {
      formLayout: 'horizontal',
      model: {},
      rules: {
        note: [{required: true, message: 'Please input your note!'}],
        gender:[{ required: true, message: 'Please select your gender!' }]
      },
      areaOptions:[]
    }
  },
  methods: {
    handleSubmit (e) {
      e.preventDefault()
      this.$refs.form.validate((ok, err) => {
        if (ok) {
          this.$message.success('succeed!')
        }
      })
    },
    handleSelectChange (value) {
      this.model.note = `Hi, ${value === 'male' ? 'man' : 'lady'}!`
    },
    onChange(value, selectedOptions) {
    },
    filter(inputValue, path) {
      return (path.some(option => (option.label).toLowerCase().indexOf(inputValue.toLowerCase()) > -1));
    },
  },
  created (){

    getAction('/mock/api/area').then((res) => {
      this.areaOptions = res;
    })
  },
  watch: {
    $route: {
      immediate: true,
      handler() {
        let sex = this.$route.query.sex
      }
    }
  },
}
</script>