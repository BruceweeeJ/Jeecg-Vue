<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :md="12" :sm="16">
            <a-form-item label="领用日期">
              <j-date placeholder="请选择开始日期" class="query-group-cust" v-model="queryParam.equsedate_begin"></j-date>
              <span class="query-group-split-cust"></span>
              <j-date placeholder="请选择结束日期" class="query-group-cust" v-model="queryParam.equsedate_end"></j-date>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <a-form-item label="领用单位">
              <a-input placeholder="请输入领用单位" v-model="queryParam.equseunit"></a-input>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :md="6" :sm="8">
              <a-form-item label="领用责任人">
                <a-input placeholder="请输入领用责任人" v-model="queryParam.equsepeople"></a-input>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="设备管理员">
                <a-input placeholder="请输入设备管理员" v-model="queryParam.equseadmin"></a-input>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="当前借出状态">
                <a-input placeholder="请输入当前借出状态" v-model="queryParam.eqflagstate"></a-input>
              </a-form-item>
            </a-col>
            <a-col :md="12" :sm="16">
              <a-form-item label="归还日期">
                <j-date placeholder="请选择开始日期" class="query-group-cust" v-model="queryParam.eqreturndate_begin"></j-date>
                <span class="query-group-split-cust"></span>
                <j-date placeholder="请选择结束日期" class="query-group-cust" v-model="queryParam.eqreturndate_end"></j-date>
              </a-form-item>
            </a-col>
            <a-col :md="6" :sm="8">
              <a-form-item label="设备使用情况">
                <a-input placeholder="请输入设备使用情况" v-model="queryParam.equsestate"></a-input>
              </a-form-item>
            </a-col>
          </template>
          <a-col :md="6" :sm="8" >
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                {{ toggleSearchStatus ? '收起' : '展开' }}
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>

        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->
    
    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('ELEC_USEDETAIL')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{fixed:true,selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无此图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="图片不存在" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无此文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="uploadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <elecUsedetail-modal ref="modalForm" @ok="modalFormOk"></elecUsedetail-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import ElecUsedetailModal from './modules/ElecUsedetailModal'
  import JDate from '@/components/jeecg/JDate.vue'

  export default {
    name: "ElecUsedetailList",
    mixins:[JeecgListMixin],
    components: {
      JDate,
      ElecUsedetailModal
    },
    data () {
      return {
        description: 'ELEC_USEDETAIL管理页面',
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'设备ID',
            align:"center",
            dataIndex: 'eqid'
          },
          {
            title:'领用日期',
            align:"center",
            dataIndex: 'equsedate',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'领用单位',
            align:"center",
            dataIndex: 'equseunit'
          },
          {
            title:'领用责任人',
            align:"center",
            dataIndex: 'equsepeople'
          },
          {
            title:'设备管理员',
            align:"center",
            dataIndex: 'equseadmin'
          },
          {
            title:'当前借出状态',
            align:"center",
            dataIndex: 'eqflagstate'
          },
          {
            title:'归还日期',
            align:"center",
            dataIndex: 'eqreturndate',
            customRender:function (text) {
              return !text?"":(text.length>10?text.substr(0,10):text)
            }
          },
          {
            title:'设备使用情况',
            align:"center",
            dataIndex: 'equsestate'
          },
          {
            title:'备注',
            align:"center",
            dataIndex: 'eqtext'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/equipment_manage/elecUsedetail/list",
          delete: "/equipment_manage/elecUsedetail/delete",
          deleteBatch: "/equipment_manage/elecUsedetail/deleteBatch",
          exportXlsUrl: "/equipment_manage/elecUsedetail/exportXls",
          importExcelUrl: "equipment_manage/elecUsedetail/importExcel",
        },
        dictOptions:{
        },
      }
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    },
    methods: {
      initDictConfig(){
      }
       
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less'
</style>