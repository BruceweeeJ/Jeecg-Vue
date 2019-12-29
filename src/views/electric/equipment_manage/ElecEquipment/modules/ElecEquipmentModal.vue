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

        <a-form-item label="设备编号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqcode', validatorRules.eqcode]" placeholder="请输入设备编号"></a-input>
        </a-form-item>
        <a-form-item label="设备名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqname', validatorRules.eqname]" placeholder="请输入设备名称"></a-input>
        </a-form-item>
        <a-form-item label="规格型号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqmodel', validatorRules.eqmodel]" placeholder="请输入规格型号"></a-input>
        </a-form-item>
        <a-form-item label="品牌" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbrand', validatorRules.eqbrand]" placeholder="请输入品牌"></a-input>
        </a-form-item>
        <a-form-item label="用途" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equse', validatorRules.equse]" placeholder="请输入用途"></a-input>
        </a-form-item>
        <a-form-item label="输出形式" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equsetype', validatorRules.equsetype]" placeholder="请输入输出形式"></a-input>
        </a-form-item>
        <a-form-item label="设备类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqtype', validatorRules.eqtype]" placeholder="请输入设备类型"></a-input>
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

  export default {
    name: "ElecEquipmentModal",
    components: { 
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
          eqcode: {rules: [
          ]},
          eqname: {rules: [
          ]},
          eqmodel: {rules: [
          ]},
          eqbrand: {rules: [
          ]},
          equse: {rules: [
          ]},
          equsetype: {rules: [
          ]},
          eqtype: {rules: [
          ]},
          eqtext: {rules: [
          ]},
        },
        url: {
          add: "/equipment_manage/elecEquipment/add",
          edit: "/equipment_manage/elecEquipment/edit",
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
          this.form.setFieldsValue(pick(this.model,'eqcode','eqname','eqmodel','eqbrand','equse','equsetype','eqtype','eqtext'))
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
        this.form.setFieldsValue(pick(row,'eqcode','eqname','eqmodel','eqbrand','equse','equsetype','eqtype','eqtext'))
      },

      
    }
  }
</script>