<template>
  <a-drawer
    :title="title"
    :width="width"
    placement="right"
    :closable="false"
    @close="close"
    :visible="visible">
  
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
        <a-form-item label="检修情况" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqoverconditionlast', validatorRules.eqoverconditionlast]" placeholder="请输入检修情况"></a-input>
        </a-form-item>
        <a-form-item label="检修单位" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqoverunitlast', validatorRules.eqoverunitlast]" placeholder="请输入检修单位"></a-input>
        </a-form-item>
        <a-form-item label="校准日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择校准日期" v-decorator="[ 'eqadjustdatelast', validatorRules.eqadjustdatelast]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="校准状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqadjuststate', validatorRules.eqadjuststate]" placeholder="请输入校准状态"></a-input>
        </a-form-item>
        <a-form-item label="校准情况" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqadjustconditionlast', validatorRules.eqadjustconditionlast]" placeholder="请输入校准情况"></a-input>
        </a-form-item>
        <a-form-item label="校准单位" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqadjustunitlast', validatorRules.eqadjustunitlast]" placeholder="请输入校准单位"></a-input>
        </a-form-item>
        <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqtext', validatorRules.eqtext]" placeholder="请输入备注"></a-input>
        </a-form-item>
        <a-form-item label="创建人" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'createBy', validatorRules.createBy]" placeholder="请输入创建人"></a-input>
        </a-form-item>
        <a-form-item label="创建时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择创建时间" v-decorator="[ 'createTime', validatorRules.createTime]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="更新人" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'updateBy', validatorRules.updateBy]" placeholder="请输入更新人"></a-input>
        </a-form-item>
        <a-form-item label="更新日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择更新日期" v-decorator="[ 'updateTime', validatorRules.updateTime]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        
      </a-form>
    </a-spin>
    <a-button type="primary" @click="handleOk">确定</a-button>
    <a-button type="primary" @click="handleCancel">取消</a-button>
  </a-drawer>
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
          eqoverconditionlast: {rules: [
          ]},
          eqoverunitlast: {rules: [
          ]},
          eqadjustdatelast: {rules: [
          ]},
          eqadjuststate: {rules: [
          ]},
          eqadjustconditionlast: {rules: [
          ]},
          eqadjustunitlast: {rules: [
          ]},
          eqtext: {rules: [
          ]},
          createBy: {rules: [
          ]},
          createTime: {rules: [
          ]},
          updateBy: {rules: [
          ]},
          updateTime: {rules: [
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
          this.form.setFieldsValue(pick(this.model,'eqid','eqoverdatelast','eqoverstate','eqoverconditionlast','eqoverunitlast','eqadjustdatelast','eqadjuststate','eqadjustconditionlast','eqadjustunitlast','eqtext','createBy','createTime','updateBy','updateTime'))
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
        this.form.setFieldsValue(pick(row,'eqid','eqoverdatelast','eqoverstate','eqoverconditionlast','eqoverunitlast','eqadjustdatelast','eqadjuststate','eqadjustconditionlast','eqadjustunitlast','eqtext','createBy','createTime','updateBy','updateTime'))
      }
      
    }
  }
</script>

<style lang="less" scoped>
/** Button按钮间距 */
  .ant-btn {
    margin-left: 30px;
    margin-bottom: 30px;
    float: right;
  }
</style>