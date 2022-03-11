<template>
  <div>
    <div v-show='identity==2||identity==1' class="page-header-index-wide">
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

      <a-card :loading="loading" :bordered="false" :body-style="{padding: '0'}">
        <div class="salesCard">
          <a-tabs default-active-key="1" size="large" :tab-bar-style="{marginBottom: '24px', paddingLeft: '16px'}">
            <div class="extra-wrapper" slot="tabBarExtraContent">
              <div class="extra-item">
                <a>今日</a>
                <a>本周</a>
                <a>本月</a>
                <a>本年</a>
              </div>
              <a-range-picker :style="{width: '256px'}" />
            </div>
            <a-tab-pane loading="true" tab="销售额" key="1">
              <a-row>
                <a-col :xl="16" :lg="12" :md="12" :sm="24" :xs="24">
                  <bar title="销售额排行" :dataSource="barData"/>
                </a-col>
                <a-col :xl="8" :lg="12" :md="12" :sm="24" :xs="24">
                  <rank-list title="门店销售排行榜" :list="rankList"/>
                </a-col>
              </a-row>
            </a-tab-pane>
            <a-tab-pane tab="销售趋势" key="2">
              <a-row>
                <a-col :xl="16" :lg="12" :md="12" :sm="24" :xs="24">
                  <bar title="销售额趋势" :dataSource="barData"/>
                </a-col>
                <a-col :xl="8" :lg="12" :md="12" :sm="24" :xs="24">
                  <rank-list title="门店销售排行榜" :list="rankList"/>
                </a-col>
              </a-row>
            </a-tab-pane>
          </a-tabs>
        </div>
      </a-card>

      <a-row>
        <a-col :span="24">
          <a-card :loading="loading" :bordered="false" title="最近一周访问量统计" :style="{ marginTop: '24px' }">
            <a-row>
              <a-col :span="6">
                <head-info title="今日IP" :content="loginfo.todayIp"></head-info>
              </a-col>
              <a-col :span="2">
                <a-spin class='circle-cust'>
                  <a-icon slot="indicator" type="environment" style="font-size: 24px"  />
                </a-spin>
              </a-col>
              <a-col :span="6">
                <head-info title="今日访问" :content="loginfo.todayVisitCount"></head-info>
              </a-col>
              <a-col :span="2">
                <a-spin class='circle-cust'>
                  <a-icon slot="indicator" type="team" style="font-size: 24px"  />
                </a-spin>
              </a-col>
              <a-col :span="6">
                <head-info title="总访问量" :content="loginfo.totalVisitCount"></head-info>
              </a-col>
              <a-col :span="2">
                <a-spin class='circle-cust'>
                  <a-icon slot="indicator" type="rise" style="font-size: 24px"  />
                </a-spin>
              </a-col>
            </a-row>
            <line-chart-multid :fields="visitFields" :dataSource="visitInfo"></line-chart-multid>
          </a-card>
        </a-col>
      </a-row>
    </div>
    <div>
      <div v-show='identity==4'>
        <a-row :gutter="24">
          <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
            <chart-card  title="余额" total='￥ 0.00'>
              <a-tooltip title="余额" slot="action" >
                <a-icon type="info-circle-o" />
              </a-tooltip>
            </chart-card>
          </a-col>
          <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
            <chart-card  title="待核销欠款" total='￥ 0.00'>
              <a-tooltip title="待核销欠款" slot="action">
                <a-icon type="info-circle-o" />
              </a-tooltip>

            </chart-card>
          </a-col>
          <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
            <chart-card  title="待付账单" total="￥ 0.00">
              <a-tooltip title="待付账单" slot="action">
                <a-icon type="info-circle-o" />
              </a-tooltip>
            </chart-card>
          </a-col>
          <a-col :sm="24" :md="12" :xl="6" :style="{ marginBottom: '24px' }">
            <chart-card title="待出账单" total="￥ 0.00">
              <a-tooltip title="待出账单" slot="action">
                <a-icon type="info-circle-o" />
              </a-tooltip>
            </chart-card>
          </a-col>
        </a-row>
      </div>
      <div v-show='identity==4' class="page-header-index-wide">
        <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
           <span style='font-size: 125%'>运单</span>
        </div>
        <a-col :sm="24" :md="12" :xl="4" :style="{ marginBottom: '24px' }">
          <a-card title="已下单"  >
            <a slot="extra" href="#" @click='changePage("已下单")'>>></a>
            <p style='font-size: 35px;font-family: Arial;color: black'>{{userStatics.ordered }}</p>
          </a-card>
        </a-col>

        <a-col :sm="24" :md="12" :xl="4" :style="{ marginBottom: '24px' }">
          <a-card title="已收货"  >
            <a slot="extra" href="#" @click='changePage("已收货")' >>></a>
            <p style='font-size: 35px;font-family: Arial;color: black'>{{userStatics.received}}</p>
          </a-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="4" :style="{ marginBottom: '24px'}">
          <a-card title="转运中"  >
            <a slot="extra" href="#"  @click='changePage("转运中")'>>></a>
            <p style='font-size: 35px;font-family: Arial;color: black'>{{userStatics.transit}}</p>
          </a-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="4" :style="{ marginBottom: '24px' }">
          <a-card title="已签收"  >
            <a slot="extra" href="#"  @click='changePage("已签收")'>>></a>
            <p style='font-size: 35px;font-family: Arial;color: black'>{{userStatics.sign}}</p>
          </a-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="4" :style="{ marginBottom: '24px' }">
          <a-card title="退件"  >
            <a slot="extra" href="#"  @click='changePage("退件")'>>></a>
            <p style='font-size: 35px;font-family: Arial;color: black'>{{userStatics.return}}</p>
          </a-card>
        </a-col>
        <a-col :sm="24" :md="12" :xl="4" :style="{ marginBottom: '24px' }">
          <a-card title="取消"  >
            <a slot="extra" href="#"  @click='changePage("取消")'>>></a>
            <p style='font-size: 35px;font-family: Arial;color: black'>{{userStatics.cancel}}</p>
          </a-card>
        </a-col>
    </div>

      <a-alert
        v-if ="identity==0"
        message="温馨提示！"
        description="请先在个人信息页完成认证.."
        type="warning"
        show-icon
      />
      <a-alert
        v-if ="identity==3"
        message="温馨提示！"
        description="请先在个人信息页重新提交完善后的认证资料"
        type="warning"
        show-icon
      />
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
  import { handleDetailss ,getAction} from '../../api/manage'
  import { mapGetters } from 'vuex'

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
    name: "IndexChart",
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
        loading: false,
        center: null,
        rankList,
        barData,
        loginfo:{},
        visitFields:['ip','visit'],
        visitInfo:[],
        indicator: <a-icon type="loading" style="font-size: 24px" spin />,
        identity:0,
        totalWaybills:0,
        totalBill:0,
        userStatics:{"ordered":0,"received":0,"transit":0,"sign":0,"return":0,"cancel":0},
      }
    },
    created() {
      this.getStatus();
      this.initLogInfo();
      this.getTotalWaybills();
      this.getUserInfo();
      this.getStatics();
    },
    methods: {
      ...mapGetters(["userInfo"]),
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
      getUserInfo(){


      },
      async getStatics(){
        this.userStatics =await getAction("/zmexpress/zmWaybills/getSortNum",{name:this.userInfo().username}).then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      changePage(val){
        this.$router.push({path: '/src/views/zm/waybills/ZmWaybillsList',query:{status: val}})
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
      async getStatus(){
        let params = this.userInfo().clientId;
        var data =await handleDetailss( '/zmexpress/zmUserDetail/queryByUserCode', {userCode: params}).then(res => {
          if (res.success) {
            return res.result;
          }
        });
        if (!this.isEmpty(data.status))
           this.identity = data.status;
        else{
          this.identity = this.userInfo().userIdentity;
        }
      },
      isEmpty:function(v) {
        switch (typeof v) {
          case 'undefined':
            return true;
          case 'string':
            if (v.replace(/(^[ \t\n\r]*)|([ \t\n\r]*$)/g, '').length == 0) return true;
            break;
          case 'boolean':
            if (!v) return true;
            break;
          case 'number':
            if (0 === v || isNaN(v)) return true;
            break;
          case 'object':
            if (null === v || v.length === 0) return true;
            for (var i in v) {
              return false;
            }
            return true;
        }
        return false;
      }
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