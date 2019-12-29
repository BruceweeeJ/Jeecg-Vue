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

        <a-form-item label="电池ID" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatid', validatorRules.eqbatid]" placeholder="请输入电池ID"></a-input>
        </a-form-item>
        <a-form-item label="充电日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择充电日期" v-decorator="[ 'eqchardate', validatorRules.eqchardate]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="充电时长" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqchartime', validatorRules.eqchartime]" placeholder="请输入充电时长" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="电池管理员" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatadmin', validatorRules.eqbatadmin]" placeholder="请输入电池管理员"></a-input>
        </a-form-item>
        <a-form-item label="电池使用状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equesstate', validatorRules.equesstate]" placeholder="请输入电池使用状态"></a-input>
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
    name: "ElecBatterychargeModal",
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
          eqbatid: {rules: [
          ]},
          eqchardate: {rules: [
          ]},
          eqchartime: {rules: [
          ]},
          eqbatadmin: {rules: [
          ]},
          equesstate: {rules: [
          ]},
        },
        url: {
          add: "/equipment_manage/elecBatterycharge/add",
          edit: "/equipment_manage/elecBatterycharge/edit",
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
          this.form.setFieldsValue(pick(this.model,'eqbatid','eqchardate','eqchartime','eqbatadmin','equesstate'))
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
        this.form.setFieldsValue(pick(row,'eqbatid','eqchardate','eqchartime','eqbatadmin','equesstate'))
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