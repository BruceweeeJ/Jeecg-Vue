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

        <a-form-item label="设备编号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqcode', validatorRules.eqcode]" placeholder="请输入设备编号"></a-input>
        </a-form-item>
        <a-form-item label="设备名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqname', validatorRules.eqname]" placeholder="请输入设备名称"></a-input>
        </a-form-item>
        <a-form-item label="规格型号" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqmodel', validatorRules.eqmodel]" placeholder="请输入规格型号"></a-input>
        </a-form-item>
        <a-form-item label="产地" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqarea', validatorRules.eqarea]" placeholder="请输入产地"></a-input>
        </a-form-item>
        <a-form-item label="厂家" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqvender', validatorRules.eqvender]" placeholder="请输入厂家"></a-input>
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
        <a-form-item label="出厂日期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-date placeholder="请选择出厂日期" v-decorator="[ 'eqdate', validatorRules.eqdate]" :trigger-change="true" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="使用年限" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqlimitdate', validatorRules.eqlimitdate]" placeholder="请输入使用年限" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="保修时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqwarrantydate', validatorRules.eqwarrantydate]" placeholder="请输入保修时间" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="使用温度（下限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'equsetemlow', validatorRules.equsetemlow]" placeholder="请输入使用温度（下限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="使用温度（上限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'equsetemhigh', validatorRules.equsetemhigh]" placeholder="请输入使用温度（上限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="使用湿度（下限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'equsewetlow', validatorRules.equsewetlow]" placeholder="请输入使用湿度（下限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="使用湿度（上限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'equsewethigh', validatorRules.equsewethigh]" placeholder="请输入使用湿度（上限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="使用天气" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'equseweather', validatorRules.equseweather]" placeholder="请输入使用天气"></a-input>
        </a-form-item>
        <a-form-item label="存储温度（下限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqstoretemlow', validatorRules.eqstoretemlow]" placeholder="请输入存储温度（下限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="存储温度（上限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqstoretemhigh', validatorRules.eqstoretemhigh]" placeholder="请输入存储温度（上限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="存储湿度（下限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqstorewetlow', validatorRules.eqstorewetlow]" placeholder="请输入存储湿度（下限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="存储湿度（上限）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'eqstorewethigh', validatorRules.eqstorewethigh]" placeholder="请输入存储湿度（上限）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="售后地点" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqaftersale', validatorRules.eqaftersale]" placeholder="请输入售后地点"></a-input>
        </a-form-item>
        <a-form-item label="售后联系方式" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqaftersaletel', validatorRules.eqaftersaletel]" placeholder="请输入售后联系方式"></a-input>
        </a-form-item>
        <a-form-item label="检修周期" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqoverhaul', validatorRules.eqoverhaul]" placeholder="请输入检修周期"></a-input>
        </a-form-item>
        <a-form-item label="设备类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqtype', validatorRules.eqtype]" placeholder="请输入设备类型"></a-input>
        </a-form-item>
        <a-form-item label="备注" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'eqtext', validatorRules.eqtext]" placeholder="请输入备注"></a-input>
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
    name: "ElecEquipmentModal",
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
          eqcode: {rules: [
          ]},
          eqname: {rules: [
          ]},
          eqmodel: {rules: [
          ]},
          eqarea: {rules: [
          ]},
          eqvender: {rules: [
          ]},
          eqbrand: {rules: [
          ]},
          equse: {rules: [
          ]},
          equsetype: {rules: [
          ]},
          eqdate: {rules: [
          ]},
          eqlimitdate: {rules: [
          ]},
          eqwarrantydate: {rules: [
          ]},
          equsetemlow: {rules: [
          ]},
          equsetemhigh: {rules: [
          ]},
          equsewetlow: {rules: [
          ]},
          equsewethigh: {rules: [
          ]},
          equseweather: {rules: [
          ]},
          eqstoretemlow: {rules: [
          ]},
          eqstoretemhigh: {rules: [
          ]},
          eqstorewetlow: {rules: [
          ]},
          eqstorewethigh: {rules: [
          ]},
          eqaftersale: {rules: [
          ]},
          eqaftersaletel: {rules: [
          ]},
          eqoverhaul: {rules: [
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
          this.form.setFieldsValue(pick(this.model,'eqcode','eqname','eqmodel','eqarea','eqvender','eqbrand','equse','equsetype','eqdate','eqlimitdate','eqwarrantydate','equsetemlow','equsetemhigh','equsewetlow','equsewethigh','equseweather','eqstoretemlow','eqstoretemhigh','eqstorewetlow','eqstorewethigh','eqaftersale','eqaftersaletel','eqoverhaul','eqtype','eqtext'))
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
        this.form.setFieldsValue(pick(row,'eqcode','eqname','eqmodel','eqarea','eqvender','eqbrand','equse','equsetype','eqdate','eqlimitdate','eqwarrantydate','equsetemlow','equsetemhigh','equsewetlow','equsewethigh','equseweather','eqstoretemlow','eqstoretemhigh','eqstorewetlow','eqstorewethigh','eqaftersale','eqaftersaletel','eqoverhaul','eqtype','eqtext'))
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