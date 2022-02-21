<template>
  <div>
    <div class="page-header-index-wide">
      <a-row :gutter="24">
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="总运单额" :total='totalWaybills'>
            <a-tooltip title="系统总运单量" slot="action" >
              <a-icon type="info-circle-o" />
            </a-tooltip>
            <div>
              <trend flag="up" style="margin-right: 16px;">
                <span slot="term">周同比</span>
                12%
              </trend>
              <trend flag="down">
                <span slot="term">日同比</span>
                11%
              </trend>
            </div>
            <template slot="footer">日均销售额<span>￥ 234.56</span></template>
          </chart-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="总提单数" :total='totalBill'>
            <a-tooltip title="指标说明" slot="action">
              <a-icon type="info-circle-o" />
            </a-tooltip>
            <div>
              <mini-area />
            </div>
            <template slot="footer">日订单量<span> {{ '1234' | NumberFormat }}</span></template>
          </chart-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="支付笔数" :total="6560 | NumberFormat">
            <a-tooltip title="指标说明" slot="action">
              <a-icon type="info-circle-o" />
            </a-tooltip>
            <div>
              <mini-bar :height="40" />
            </div>
            <template slot="footer">待支付订单数 <span>60</span></template>
          </chart-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
          <chart-card :loading="loading" title="已完成订单数" total="78">
            <a-tooltip title="指标说明" slot="action">
              <a-icon type="info-circle-o" />
            </a-tooltip>
            <div>
              <mini-progress color="rgb(19, 194, 194)" :target="80" :percentage="78" :height="8" />
            </div>
            <template slot="footer">
              <trend flag="down" style="margin-right: 16px;">
                <span slot="term">同周比</span>
                12%
              </trend>
              <trend flag="up">
                <span slot="term">日环比</span>
                80%
              </trend>
            </template>
          </chart-card>
        </a-col>
      </a-row>


    </div>
    <div>
<!--      <a-card :bordered="false">-->
<!--        <a-row>-->
<!--          <a-col :sm="8" :xs="24">-->
<!--            <head-info title="我的待办" content="8个任务" :bordered="true"/>-->
<!--          </a-col>-->
<!--          <a-col :sm="8" :xs="24">-->
<!--            <head-info title="本周任务平均处理时间" content="32分钟" :bordered="true"/>-->
<!--          </a-col>-->
<!--          <a-col :sm="8" :xs="24">-->
<!--            <head-info title="本周完成任务数" content="24个"/>-->
<!--          </a-col>-->
<!--        </a-row>-->
<!--      </a-card>-->

      <a-card
        style="margin-top: 24px"
        :bordered="false"
        title="标准列表">

        <div slot="extra">
          <a-radio-group>
            <a-radio-button>按用户统计</a-radio-button>
            <a-radio-button>按客户统计</a-radio-button>
            <a-radio-button>等待中</a-radio-button>
          </a-radio-group>
          <a-input-search style="margin-left: 16px; width: 272px;" />
        </div>

        <div class="operate">
          <a-button type="dashed" style="width: 100%" icon="plus">添加</a-button>
        </div>

        <a-list size="large" :pagination="{showSizeChanger: true, showQuickJumper: true, pageSize: 5, total: 50}">
          <a-list-item :key="index" v-for="(item, index) in data">
            <a-list-item-meta :description="item.description">
              <a-avatar slot="avatar" size="large" shape="square" :src="item.avatar"/>
              <a slot="title">{{ item.title }}</a>
            </a-list-item-meta>
            <div slot="actions">
              <a>编辑</a>
            </div>
            <div slot="actions">
              <a-dropdown>
                <a-menu slot="overlay">
                  <a-menu-item><a>编辑</a></a-menu-item>
                  <a-menu-item><a>删除</a></a-menu-item>
                </a-menu>
                <a>更多<a-icon type="down"/></a>
              </a-dropdown>
            </div>
            <div class="list-content">
              <div class="list-content-item">
                <span>订单数</span>
                <p>{{ item.owner }}</p>
              </div>
              <div class="list-content-item">
                <span>开始时间</span>
                <p>{{ item.startAt }}</p>
              </div>
              <div class="list-content-item">
                <a-progress :percent="item.progress.value" :status="!item.progress.status ? null : item.progress.status" style="width: 180px" />
              </div>
            </div>
          </a-list-item>
        </a-list>

      </a-card>
    </div>
  </div>

</template>

<script>
import ChartCard from '@/components/ChartCard'
import ACol from "ant-design-vue/es/grid/Col"
import ATooltip from "ant-design-vue/es/tooltip/Tooltip"
import MiniArea from '@/components/chart/MiniArea'
import MiniBar from '@/components/chart/MiniBar'
import MiniProgress from '@/components/chart/MiniProgress'
import RankList from '@/components/chart/RankList'
import Bar from '@/components/chart/Bar'
import LineChartMultid from '@/components/chart/LineChartMultid'
import HeadInfo from '@/components/tools/HeadInfo.vue'

import Trend from '@/components/Trend'
import { getLoginfo,getVisitInfo } from '@/api/api'
import { handleDetailss ,getAction} from '../../../api/manage'

const data = []
data.push({
  title: 'Alipay',
  avatar: 'https://gw.alipayobjects.com/zos/rmsportal/WdGqmHpayyMjiEhcKoVE.png',
  description: '那是一种内在的东西， 他们到达不了，也无法触及的',
  owner: '付晓晓',
  startAt: '2018-07-26 22:44',
  progress: {
    value: 90
  }
})
data.push({
  title: 'Angular',
  avatar: 'https://gw.alipayobjects.com/zos/rmsportal/zOsKZmFRdUtvpqCImOVY.png',
  description: '希望是一个好东西，也许是最好的，好东西是不会消亡的',
  owner: '曲丽丽',
  startAt: '2018-07-26 22:44',
  progress: {
    value: 54
  }
})
data.push({
  title: 'Ant Design',
  avatar: 'https://gw.alipayobjects.com/zos/rmsportal/dURIMkkrRFpPgTuzkwnB.png',
  description: '生命就像一盒巧克力，结果往往出人意料',
  owner: '林东东',
  startAt: '2018-07-26 22:44',
  progress: {
    value: 66
  }
})
data.push({
  title: 'Ant Design Pro',
  avatar: 'https://gw.alipayobjects.com/zos/rmsportal/sfjbOqnsXXJgNCjCzDBL.png',
  description: '城镇中有那么多的酒馆，她却偏偏走进了我的酒馆',
  owner: '周星星',
  startAt: '2018-07-26 22:44',
  progress: {
    value: 30
  }
})
data.push({
  title: 'Bootstrap',
  avatar: 'https://gw.alipayobjects.com/zos/rmsportal/siCrBXXhmvTQGWPNLBow.png',
  description: '那时候我只会想自己想要什么，从不想自己拥有什么',
  owner: '吴加好',
  startAt: '2018-07-26 22:44',
  progress: {
    status: 'exception',
    value: 100
  }
})

const rankList = []
for (let i = 0; i < 7; i++) {
  rankList.push({
    name: '白鹭岛 ' + (i+1) + ' 号店',
    total: 1234.56 - i * 100
  })
}
const barData = []
for (let i = 0; i < 12; i += 1) {
  barData.push({
    x: `${i + 1}月`,
    y: Math.floor(Math.random() * 1000) + 200
  })
}
export default {
  name: 'WaybillStatics',
  components: {
    ATooltip,
    ACol,
    ChartCard,
    MiniArea,
    MiniBar,
    MiniProgress,
    RankList,
    Bar,
    Trend,
    LineChartMultid,
    HeadInfo
  },
  data() {
    return {
      loading: true,
      center: null,
      rankList,
      barData,
      loginfo:{},
      visitFields:['ip','visit'],
      visitInfo:[],
      indicator: <a-icon type="loading" style="font-size: 24px" spin />,
      data,
      totalWaybills:null,
      totalBill:null,
    }
  },
  created() {
    setTimeout(() => {
      this.loading = !this.loading
    }, 1000)
    this.initLogInfo();
    this.getTotalWaybills();
  },
  methods: {
    async getTotalWaybills (){
      var list = '/zmexpress/zmWaybills/getTotalWaybill'
      this.totalWaybills =await getAction(list,{}).then(res => {
        if (res.success) {
          return res.result;
        }
      })
      this.totalBill =await getAction("/zmexpress/zmBillloading/getTotalBill",{}).then(res => {
        if (res.success) {
          return res.result;
        }
      })
    },

    initLogInfo () {
      getLoginfo(null).then((res)=>{
        if(res.success){
          Object.keys(res.result).forEach(key=>{
            res.result[key] =res.result[key]+""
          })
          this.loginfo = res.result;
        }
      })
      getVisitInfo().then(res=>{
        if(res.success){
          this.visitInfo = res.result;
        }
      })
    },
  }
}
</script>

<style lang="less" scoped>
.ant-avatar-lg {
  width: 48px;
  height: 48px;
  line-height: 48px;
}

.list-content-item {
  color: rgba(0, 0, 0, .45);
  display: inline-block;
  vertical-align: middle;
  font-size: 14px;
  margin-left: 40px;
  span {
    line-height: 20px;
  }
  p {
    margin-top: 4px;
    margin-bottom: 0;
    line-height: 22px;
  }
}
.circle-cust{
  position: relative;
  top: 28px;
  left: -100%;
}
.extra-wrapper {
  line-height: 55px;
  padding-right: 24px;

  .extra-item {
    display: inline-block;
    margin-right: 24px;

    a {
      margin-left: 24px;
    }
  }
}

/* 首页访问量统计 */
.head-info {
  position: relative;
  text-align: left;
  padding: 0 32px 0 0;
  min-width: 125px;

  &.center {
    text-align: center;
    padding: 0 32px;
  }

  span {
    color: rgba(0, 0, 0, .45);
    display: inline-block;
    font-size: .95rem;
    line-height: 42px;
    margin-bottom: 4px;
  }
  p {
    line-height: 42px;
    margin: 0;
    a {
      font-weight: 600;
      font-size: 1rem;
    }
  }
}
</style>