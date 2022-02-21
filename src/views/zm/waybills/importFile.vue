<template>
  <a-modal
    title="导入运单"
    :width="600"
    :visible="visible"
    :confirmLoading="uploading"
    @cancel="handleClose">

    <a-form
      id="components-form-demo-validate-other"
      v-bind="formItemLayout"
    >
      <a-form-item label="选择文件">
        <a-upload
          name="file"
          :multiple="true"
          :fileList="fileList"
          :remove="handleRemove"
          :beforeUpload="beforeUpload">
          <a-button>
            <a-icon type="upload" />
            选择导入文件
          </a-button>
        </a-upload>
      </a-form-item>

    </a-form>


    <template slot="footer">
      <a-button @click="handleClose">关闭</a-button>
      <a-button
        type="primary"
        @click="handleImport"
        :disabled="fileList.length === 0"
        :loading="uploading">
        {{ uploading ? '上传中...' : '开始上传' }}
      </a-button>
    </template>

  </a-modal>
</template>

<script>
import { postAction } from '@/api/manage'
export default {
  name: 'importFile',
  props:{
    url:{
      type: String,
      default: '',
      required: false
    },
    biz:{
      type: String,
      default: '',
      required: false
    },
    //是否online导入
    online:{
      type: Boolean,
      default: false,
      required: false
    },
    importId: {
      type:String,
      default:''
    }
  },
  data(){
    return {
      formItemLayout: {
        labelCol: { span: 6 },
        wrapperCol: { span: 14 },
      },
      visible:false,
      uploading:false,
      fileList:[],
      uploadAction:'',
      foreignKeys:'',
      id:'',
      name:'',
      validateStatus: 0
    }
  },
  watch: {
    url (val) {
      if(val){
        this.uploadAction = window._CONFIG['domianURL']+val
      }
    },
    importId:{
      handler(newVal,oldVal){
        this.id = newVal
      },
      immediate:true
    }

  },
  created () {
    this.uploadAction = window._CONFIG['domianURL']+this.url
  },

  methods:{
    handleClose(){
      this.visible=false
    },
    show(arg){
      this.fileList = []
      this.uploading = false
      this.visible = true
      this.foreignKeys = arg;
      this.validateStatus = 0
    },
    handleRemove(file) {
      this.$message.warning(`移除当前${file.name}运单，请重新选择运单上传！`);
      const index = this.fileList.indexOf(file);
      const newFileList = this.fileList.slice();
      newFileList.splice(index, 1);
      this.fileList = newFileList
    },
    beforeUpload(file) {
      this.fileList = [...this.fileList, file]
      return false;
    },
    handleImport() {
      const { fileList } = this;
      const formData = new FormData();
      if(this.biz){
        formData.append('isSingleTableImport',this.biz);
      }
      if(this.foreignKeys && this.foreignKeys.length>0){
        formData.append('foreignKeys',this.foreignKeys);
      }
      if(this.online==true){
        formData.append('validateStatus',this.validateStatus);
      }
      if (this.id!=null){
        formData.append("waybillId",this.id);
      }
      fileList.forEach((file) => {
        formData.append('files[]', file);
      });
      this.uploading = true
      postAction(this.uploadAction, formData).then((res) => {
        this.uploading = false
        if(res.success){
          if(res.code == 201){
            this.errorTip(res.message, res.result)
          }else{
            this.$message.success(res.message)
          }
          this.visible=false
          this.$emit('ok')
        }else{
          this.$message.warning(res.message)
        }
      })
    },
    // 是否开启校验 开关改变事件
    handleChangeValidateStatus(checked){
      this.validateStatus = checked==true?1:0
    },
    // 错误信息提示
    errorTip(tipMessage, fileUrl) {
      const h = this.$createElement;
      let href = window._CONFIG['domianURL'] + fileUrl
      this.$warning({
        title: '导入成功,但是有错误数据!',
        content: h('div', {}, [
          h('div', tipMessage),
          h('span', '具体详情请 '),
          h('a', {
            attrs: {
              href: href,
              target: '_blank'
            },
          },'点击下载'),
        ]),
        onOk() {},
      });
    },

  }
}
</script>

<style scoped>

</style>