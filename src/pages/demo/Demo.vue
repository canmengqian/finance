<template>
  <a-card>
    <div :class="advanced ? 'search' : null">
      <a-form layout="horizontal">
        <div :class="advanced ? null: 'fold'">
          <a-row >
          <a-col :md="8" :sm="24" >
            <a-form-item
              label="站点名称"
              :labelCol="{span: 5}"
              :wrapperCol="{span: 18, offset: 1}"
            >
              <a-input placeholder="请输入" />
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="24" >
            <a-form-item
              label="状态"
              :labelCol="{span: 5}"
              :wrapperCol="{span: 18, offset: 1}"
            >
              <a-select placeholder="请选择">
                <a-select-option value="1">关闭</a-select-option>
                <a-select-option value="2">运行中</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
        </a-row>
          <a-row v-if="advanced">
          <a-col :md="8" :sm="24" >
            <a-form-item
              label="录入日期"
              :labelCol="{span: 5}"
              :wrapperCol="{span: 18, offset: 1}"
            >
              <a-date-picker style="width: 100%" placeholder="请输入录入日期" />
            </a-form-item>
          </a-col>
        </a-row>
        </div>
        <span style="float: right; margin-top: 3px;">
          <a-button type="primary">查询</a-button>
          <a-button style="margin-left: 8px">重置</a-button>
          <a @click="toggleAdvanced" style="margin-left: 8px">
            {{advanced ? '收起' : '展开'}}
            <a-icon :type="advanced ? 'up' : 'down'" />
          </a>
        </span>
      </a-form>
    </div>
    <div>
      <a-button type="primary">
        Primary
      </a-button>
      <a-button>Default</a-button>
      <a-button type="dashed">
        Dashed
      </a-button>
      <a-button type="danger">
        Danger
      </a-button>
      <a-config-provider :auto-insert-space-in-button="false">
        <a-button type="primary">
          按钮
        </a-button>
      </a-config-provider>
      <a-button type="primary">
        按钮
      </a-button>
      <a-button type="link">
        Link
      </a-button>
    </div>
<!--对话框    -->
    <div>
      <a-modal v-model="visible" title="Basic Modal" @ok="handleOk" id="websiteAdd">

      </a-modal>
    </div>
<!--  新增页面结束  -->
    <div>
      <a-space class="operator">
        <a-button @click="addNew" type="primary">新建</a-button>
        <a-button @click="showModal" type="primary">录入网站信息</a-button>
        <a-button >批量操作</a-button>
        <a-dropdown>
          <a-menu @click="handleMenuClick" slot="overlay">
            <a-menu-item key="delete">删除</a-menu-item>
            <a-menu-item key="audit">审批</a-menu-item>
          </a-menu>
          <a-button>
            更多操作 <a-icon type="down" />
          </a-button>
        </a-dropdown>
      </a-space>
      <standard-table
        :columns="columns"
        :dataSource="dataSource"
        :selectedRows.sync="selectedRows"
        @clear="onClear"
        @change="onChange"
        @selectedRowChange="onSelectChange"
      >
        <div slot="description" slot-scope="{text}">
          {{text}}
        </div>
        <div slot="action" slot-scope="{text, record}">
          <a style="margin-right: 8px" @click="openlayer">
            <a-icon type="plus"/>新增
          </a>
          <a style="margin-right: 8px">
            <a-icon type="edit"/>编辑
          </a>
          <a @click="deleteRecord(record.key)">
            <a-icon type="delete" />删除1
          </a>
          <a @click="deleteRecord(record.key)" v-auth="`delete`">
            <a-icon type="delete" />删除2
          </a>
          <router-link :to="`/list/query/detail/${record.key}`" >详情</router-link>
        </div>
        <template slot="statusTitle">
          <a-icon @click.native="onStatusTitleClick" type="info-circle" />
        </template>
      </standard-table>
    </div>
  </a-card>
</template>

<script>
import StandardTable from '@/components/table/StandardTable'
import {Button} from 'ant-design-vue'


const columns = [
  {
    title: '网站编码',
    dataIndex: 'id',
    colSpan:0 // 隐藏列
  },
  {
    title: '网站名称',
    dataIndex: 'name'
  },
  {
    title: '网站链接',
    dataIndex: 'webLink',
    scopedSlots: { customRender: 'description' }
  },
  {
    title: '网站类型',
    dataIndex: 'type',
    sorter: true,
    needTotal: true,
    customRender: (text) => text + ' 次'
  },
  {
    title: '网站描述',
    dataIndex: 'desc',
    needTotal: false,
    sorter: true,
  },
  {
    title: '是否可用',
    dataIndex: 'status',
    needTotal: false,
    sorter: true,
  },
  /*{
    dataIndex: 'status',
    needTotal: true,
    slots: {title: 'statusTitle'}
  },*/
  {
    title: '操作',
    scopedSlots: { customRender: 'action' }
  }
]

const dataSource = []

for (let i = 0; i < 100; i++) {
  dataSource.push({
    key: i,
    id: 'NO ' + i,
    name:'网站'+i,
    webLink: '链接',
    type: '金融类',
    desc: '描述'+i,
    status:0,
    updatedAt: '2018-07-26'
  })
}

export default {
  name: 'QueryList',
  // eslint-disable-next-line vue/no-unused-components
  components: {StandardTable,Button},
  data () {
    return {
      advanced: true,
      columns: columns,
      dataSource: dataSource,
      selectedRows: [],
      visible: false,
    }
  },
  authorize: {
    deleteRecord: 'delete'
  },
  methods: {
    showModal() {
      this.visible = true;
    },
    handleOk(e) {
      console.log(e);
      this.visible = false;
    },
    openlayer() {
      let self = this;
      this.$confirm({
        title: '确认提示',
        content: `是否确认删除？`,
        okButtonProps: {
          color: this.themeColor,
        },
        onOk () {
          //已选择的行，取id
          self.selectedRows = self.selectedRows.map((item) => item.id);
          self.deleteUser(self.selectedRows);
        }
      });
    },
    deleteRecord(key) {
      this.dataSource = this.dataSource.filter(item => item.key !== key)
      this.selectedRows = this.selectedRows.filter(item => item.key !== key)
    },
    toggleAdvanced () {
      this.advanced = !this.advanced
    },
    remove () {
      this.dataSource = this.dataSource.filter(item => this.selectedRows.findIndex(row => row.key === item.key) === -1)
      this.selectedRows = []
    },
    onClear() {
      this.$message.info('您清空了勾选的所有行')
    },
    onStatusTitleClick() {
      this.$message.info('你点击了状态栏表头')
    },
    onChange() {
      this.$message.info('表格状态改变了')
    },
    onSelectChange() {
      this.$message.info('选中行改变了')
    },
    addNew () {
      this.dataSource.unshift({
        key: this.dataSource.length,
        no: 'NO ' + this.dataSource.length,
        description: '这是一段描述',
        callNo: Math.floor(Math.random() * 1000),
        status: Math.floor(Math.random() * 10) % 4,
        updatedAt: '2018-07-26'
      })
    },
    handleMenuClick (e) {
      if (e.key === 'delete') {
        this.remove()
      }
    }
  }
}
</script>

<style lang="less" scoped>
  .search{
    margin-bottom: 54px;
  }
  .fold{
    width: calc(100% - 216px);
    display: inline-block
  }
  .operator{
    margin-bottom: 18px;
  }
  @media screen and (max-width: 900px) {
    .fold {
      width: 100%;
    }
  }
</style>
