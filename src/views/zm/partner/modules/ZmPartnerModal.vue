<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    switchFullscreen
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form-model ref="form" :model="model" :rules="validatorRules">
        <a-form-model-item label="父级节点" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="pid">
          <j-tree-select
            ref="treeSelect"
            placeholder="请选择父级节点"
            v-model="model.pid"
            dict="zm_partner,role,id"
            pidField="pid"
            pidValue="0"
            hasChildField="has_child"
            >
          </j-tree-select>
        </a-form-model-item>
        <a-form-model-item label="角色" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="role">
          <j-dict-select-tag type="list" v-model="model.role"  dictCode="role" placeholder="请选择角色" />
        </a-form-model-item>
        <a-form-model-item label="名字或者公司名称" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="name">
          <a-input v-model="model.name" placeholder="请输入名字或者公司名称" ></a-input>
        </a-form-model-item>
        <a-form-model-item label="电话" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="tel">
          <a-input v-model="model.tel" placeholder="请输入电话" ></a-input>
        </a-form-model-item>
        <a-form-model-item label="地址" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="address">
          <a-input v-model="model.address" placeholder="请输入地址" ></a-input>
        </a-form-model-item>
        
      </a-form-model>
    </a-spin>
  </j-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import { validateDuplicateValue } from '@/utils/util'
  export default {
    name: "ZmPartnerModal",
    components: { 
    },
    data () {
      return {
        title:"操作",
        width:800,
        visible: false,
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
          add: "/test/zmPartner/add",
          edit: "/test/zmPartner/edit",
        },
        expandedRowKeys:[],
        pidField:"pid"
     
      }
    },
    created () {
       //备份model原始值
       this.modelDefault = JSON.parse(JSON.stringify(this.model));
    },
    methods: {
      add (obj) {
        this.modelDefault.pid=''
        this.edit(Object.assign(this.modelDefault , obj));
      },
      edit (record) {
        this.model = Object.assign({}, record);
        this.visible = true;
      },
      close () {
        this.$emit('close');
        this.visible = false;
        this.$refs.form.clearValidate()
      },
      handleOk () {
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
             if(this.model.id && this.model.id === this.model[this.pidField]){
              that.$message.warning("父级节点不能选择自己");
              that.confirmLoading = false;
              return;
            }
            httpAction(httpurl,this.model,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                this.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }else{
             return false
          }
        })
      },
      handleCancel () {
        this.close()
      },
      submitSuccess(formData,flag){
        if(!formData.id){
          let treeData = this.$refs.treeSelect.getCurrTreeData()
          this.expandedRowKeys=[]
          this.getExpandKeysByPid(formData[this.pidField],treeData,treeData)
          this.$emit('ok',formData,this.expandedRowKeys.reverse());
        }else{
          this.$emit('ok',formData,flag);
        }
      },
      getExpandKeysByPid(pid,arr,all){
        if(pid && arr && arr.length>0){
          for(let i=0;i<arr.length;i++){
            if(arr[i].key==pid){
              this.expandedRowKeys.push(arr[i].key)
              this.getExpandKeysByPid(arr[i]['parentId'],all,all)
            }else{
              this.getExpandKeysByPid(pid,arr[i].children,all)
            }
          }
        }
      }
      
      
    }
  }
</script>