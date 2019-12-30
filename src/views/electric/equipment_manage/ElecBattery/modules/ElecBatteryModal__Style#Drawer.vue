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
        <a-form-item label="电池编号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatcode', validatorRules.eqbatcode]" placeholder="请输入电池编号"></a-input>
        </a-form-item>
        <a-form-item label="电池名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatname', validatorRules.eqbatname]" placeholder="请输入电池名称"></a-input>
        </a-form-item>
        <a-form-item label="电池型号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatmodel', validatorRules.eqbatmodel]" placeholder="请输入电池型号"></a-input>
        </a-form-item>
        <a-form-item label="额定电压" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqbatvoltage', validatorRules.eqbatvoltage]" placeholder="请输入额定电压" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="电池容量" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqbatcapacity', validatorRules.eqbatcapacity]" placeholder="请输入电池容量" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="充电周期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqbatcycle', validatorRules.eqbatcycle]" placeholder="请输入充电周期" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="上次充电日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择上次充电日期" v-decorator="[ 'eqchargedate', validatorRules.eqchargedate]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="电池充电状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqchargestate', validatorRules.eqchargestate]" placeholder="请输入电池充电状态"></a-input>
        </a-form-item>
        <a-form-item label="电池使用状态" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatuesstate', validatorRules.eqbatuesstate]" placeholder="请输入电池使用状态"></a-input>
        </a-form-item>
        <a-form-item label="电池充电详情" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqbatcharge', validatorRules.eqbatcharge]" placeholder="请输入电池充电详情"></a-input>
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
    name: "ElecBatteryModal",
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
          eqbatcode: {rules: [
          ]},
          eqbatname: {rules: [
          ]},
          eqbatmodel: {rules: [
          ]},
          eqbatvoltage: {rules: [
          ]},
          eqbatcapacity: {rules: [
          ]},
          eqbatcycle: {rules: [
          ]},
          eqchargedate: {rules: [
          ]},
          eqchargestate: {rules: [
          ]},
          eqbatuesstate: {rules: [
          ]},
          eqbatcharge: {rules: [
          ]},
        },
        url: {
          add: "/equipment_manage/elecBattery/add",
          edit: "/equipment_manage/elecBattery/edit",
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
          this.form.setFieldsValue(pick(this.model,'eqid','eqbatcode','eqbatname','eqbatmodel','eqbatvoltage','eqbatcapacity','eqbatcycle','eqchargedate','eqchargestate','eqbatuesstate','eqbatcharge'))
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
        this.form.setFieldsValue(pick(row,'eqid','eqbatcode','eqbatname','eqbatmodel','eqbatvoltage','eqbatcapacity','eqbatcycle','eqchargedate','eqchargestate','eqbatuesstate','eqbatcharge'))
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