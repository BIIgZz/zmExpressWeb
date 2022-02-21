<template>
  <j-modal
    :title="title"
    :width="width"
    :visible="visible"
    switchFullscreen
    @ok="handleOk"
    :okButtonProps="{ class:{'jee-hidden': disableSubmit} }"
    @cancel="handleCancel"
    cancelText="关闭">
    <zm-container-standards-form ref="realForm" @ok="submitCallback" :disabled="disableSubmit"></zm-container-standards-form>
  </j-modal>
</template>

<script>

  import ZmContainerStandardsForm from './ZmContainerStandardsForm'
  export default {
    name: 'ZmContainerStandardsModal',
    components: {
      ZmContainerStandardsForm
    },
    data () {
      return {
        title:'',
        width:1280,
        visible: false,
        disableSubmit: false
      }
    },
    methods: {
      add () {
        this.visible=true
        this.$nextTick(()=>{
          this.$refs.realForm.add();
        })
      },
      edit (record) {
        this.visible=true
        this.$nextTick(()=>{
          this.$refs.realForm.edit(record);
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        this.$refs.realForm.submitForm();
      },
      submitCallback(){
        this.$emit('ok');
        this.visible = false;
      },
      handleCancel () {
        this.close()
      }
    }
  }
</script>