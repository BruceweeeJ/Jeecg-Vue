<template>
  <a-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">

        <a-form-item label="设备ID" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqid', validatorRules.eqid]" placeholder="请输入设备ID"></a-input>
        </a-form-item>
        <a-form-item label="领用日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择领用日期" v-decorator="[ 'equsedate', validatorRules.equsedate]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="领用单位" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equseunit', validatorRules.equseunit]" placeholder="请输入领用单位"></a-input>
        </a-form-item>
        <a-form-item label="领用责任人" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equsepeople', validatorRules.equsepeople]" placeholder="请输入领用责任人"></a-input>
        </a-form-item>
        <a-form-item label="设备管理员" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equseadmin', validatorRules.equseadmin]" placeholder="请输入设备管理员"></a-input>
        </a-form-item>
        <a-form-item label="当前借出状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqflagstate', validatorRules.eqflagstate]" placeholder="请输入当前借出状态"></a-input>
        </a-form-item>
        <a-form-item label="归还日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择归还日期" v-decorator="[ 'eqreturndate', validatorRules.eqreturndate]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="设备使用情况" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equsestate', validatorRules.equsestate]" placeholder="请输入设备使用情况"></a-input>
        </a-form-item>

      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'
  import JDate from '@/components/jeecg/JDate'  

  export default {
    name: "ElecUsedetailModal",
    components: { 
      JDate,
    },
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
        visible: false,
        model: {},
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
          eqid: {rules: [
          ]},
          equsedate: {rules: [
          ]},
          equseunit: {rules: [
          ]},
          equsepeople: {rules: [
          ]},
          equseadmin: {rules: [
          ]},
          eqflagstate: {rules: [
          ]},
          eqreturndate: {rules: [
          ]},
          equsestate: {rules: [
          ]},
        },
        url: {
          add: "/equipment_manage/elecUsedetail/add",
          edit: "/equipment_manage/elecUsedetail/edit",
        }
      }
    },
    created () {
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'eqid','equsedate','equseunit','equsepeople','equseadmin','eqflagstate','eqreturndate','equsestate'))
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
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
            let formData = Object.assign(this.model, values);
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }
         
        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'eqid','equsedate','equseunit','equsepeople','equseadmin','eqflagstate','eqreturndate','equsestate'))
      },

      
    }
  }
</script>