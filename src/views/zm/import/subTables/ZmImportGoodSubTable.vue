<template>
  <a-table
    rowKey="id"
    size="middle"
    bordered
    :loading="loading"
    :columns="columns"
    :dataSource="dataSource"
    :pagination="false"
  >

    <template slot="htmlSlot" slot-scope="text">
      <div v-html="text"></div>
    </template>

    <template slot="imgSlot" slot-scope="text">
      <div style="font-size: 12px;font-style: italic;">
        <span v-if="!text">无图片</span>
        <img v-else :src="getImgView(text)" alt="" style="max-width:80px;height:25px;"/>
      </div>
    </template>

    <template slot="fileSlot" slot-scope="text">
      <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
      <a-button
              v-else
              ghost
              type="primary"
              icon="download"
              size="small"
              @click="downloadFile(text)"
      >
        <span>下载</span>
      </a-button>
    </template>

  </a-table>
</template>

<script>
  import { getAction } from '@api/manage'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'

  export default {
    name: 'ZmImportGoodSubTable',
    mixins: [JeecgListMixin],
    props: {
      record: {
        type: Object,
        default: null,
      }
    },
    data() {
      return {
        description: '货柜详情内嵌列表',
        disableMixinCreated: true,
        loading: false,
        dataSource: [],
        columns: [
          {
            title: '货箱编号',
            align: 'center',
            dataIndex: 'caseid',
          },
          {
            title: '货箱重量',
            align: 'center',
            dataIndex: 'weight',
          },
          {
            title: '货箱长度',
            align: 'center',
            dataIndex: 'length',
          },
          {
            title: '货箱宽度',
            align: 'center',
            dataIndex: 'width',
          },
          {
            title: '货箱高度',
            align: 'center',
            dataIndex: 'height',
          },
          {
            title: '中文名称',
            align: 'center',
            dataIndex: 'cnName',
          },
          {
            title: '英文名称',
            align: 'center',
            dataIndex: 'enName',
          },
          {
            title: '申报单价',
            align: 'center',
            dataIndex: 'declaredPrice',
          },
          {
            title: '申报数量',
            align: 'center',
            dataIndex: 'declaredNumber',
          },
          {
            title: '产品海关编码',
            align: 'center',
            dataIndex: 'hscode',
          },
          {
            title: '产品材料',
            align: 'center',
            dataIndex: 'material',
          },
          {
            title: '产品用途',
            align: 'center',
            dataIndex: 'application',
          },
          {
            title: '品牌',
            align: 'center',
            dataIndex: 'brand',
          },
          {
            title: '品牌类型',
            align: 'center',
            dataIndex: 'type_dictText'
          },
          {
            title: '产品型号',
            align: 'center',
            dataIndex: 'model',
          },
          {
            title: '销售链接',
            align: 'center',
            dataIndex: 'link',
          },
          {
            title: '产品售价',
            align: 'center',
            dataIndex: 'price',
          },
          {
            title: '图片链接',
            align: 'center',
            dataIndex: 'picture',
            scopedSlots: {customRender: 'imgSlot'}
          },
        ],
        url: {
          listByMainId: '/zmexpress/zmImportFba/queryZmImportGoodByMainId',
        },
      }
    },
    watch: {
      record: {
        immediate: true,
        handler() {
          if (this.record != null) {
            this.loadData(this.record)
          }
        }
      }
    },
    methods: {

      loadData(record) {
        this.loading = true
        this.dataSource = []
        getAction(this.url.listByMainId, {
          id: record.id
        }).then((res) => {
          if (res.success) {
            this.dataSource = res.result.records
          }
        }).finally(() => {
          this.loading = false
        })
      },

    },
  }
</script>

<style scoped>

</style>
