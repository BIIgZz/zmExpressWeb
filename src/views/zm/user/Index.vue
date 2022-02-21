<template>
  <a-row :gutter="24">
    <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
      <chart-card :loading="loading" title="余额" total='100'>
        <a-tooltip title="余额" slot="action" >
          <a-icon type="info-circle-o" />
        </a-tooltip>
      </chart-card>
    </a-col>
    <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
      <chart-card :loading="loading" title="待核销欠款" total='100'>
        <a-tooltip title="指标说明" slot="action">
          <a-icon type="info-circle-o" />
        </a-tooltip>
      </chart-card>
    </a-col>
    <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
      <chart-card :loading="loading" title="待付账单" :total="6560 | NumberFormat">
        <a-tooltip title="指标说明" slot="action">
          <a-icon type="info-circle-o" />
        </a-tooltip>
        <div>
          <mini-bar :height="40" />
        </div>
        <template slot="footer">转化率 <span>60%</span></template>
      </chart-card>
    </a-col>
    <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
      <chart-card :loading="loading" title="运营活动效果" total="78%">
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
export default {
  name: 'Index',
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
      indicator: <a-icon type="loading" style="font-size: 24px" spin />
    }
  },
  created() {
    setTimeout(() => {
      this.loading = !this.loading
    }, 1000)
    this.initLogInfo();
  },
  methods: {
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