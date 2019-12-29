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
        <a-form-item label="检修日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择检修日期" v-decorator="[ 'eqoverdatelast', validatorRules.eqoverdatelast]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="检修状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqoverstate', validatorRules.eqoverstate]" placeholder="请输入检修状态"></a-input>
        </a-form-item>
        <a-form-item label="校准日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择校准日期" v-decorator="[ 'eqadjustdatelast', validatorRules.eqadjustdatelast]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="校准状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqadjuststate', validatorRules.eqadjuststate]" placeholder="请输入校准状态"></a-input>
        </a-form-item>
        <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqtext', validatorRules.eqtext]" placeholder="请输入备注"></a-input>
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
    name: "ElecOveradjustModal",
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
          eqoverdatelast: {rules: [
          ]},
          eqoverstate: {rules: [
          ]},
          eqadjustdatelast: {rules: [
          ]},
          eqadjuststate: {rules: [
          ]},
          eqtext: {rules: [
          ]},
        },
        url: {
          add: "/equipment_manage/elecOveradjust/add",
          edit: "/equipment_manage/elecOveradjust/edit",
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
          this.form.setFieldsValue(pick(this.model,'eqid','eqoverdatelast','eqoverstate','eqadjustdatelast','eqadjuststate','eqtext'))
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
        this.form.setFieldsValue(pick(row,'eqid','eqoverdatelast','eqoverstate','eqadjustdatelast','eqadjuststate','eqtext'))
      },

      
    }
  }
</script>