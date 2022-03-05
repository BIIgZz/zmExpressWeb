
<template>

<div>

  <a-card class="j-inner-table-wrapper" :bordered="false" v-show="identity!='0'" >

    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="运单号">
              <a-input placeholder="请输入运单号" v-model="queryParam.waybillId"></a-input>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="客户订单号">
              <a-input placeholder="请输入客户订单号" v-model="queryParam.orderId"></a-input>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="客户名称">
                <j-dict-select-tag placeholder="请选择客户名称" v-model="queryParam.username" dictCode="zm_client_main,username,username"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="用户名称">
                <j-dict-select-tag placeholder="请选择用户名称" v-model="queryParam.createBy" dictCode="sys_user,username,username"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="公司名">
                <a-input placeholder="请输入公司名" v-model="queryParam.company"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="柜号">
                <a-input placeholder="请输入柜号" v-model="queryParam.caseId"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="FBA号">
                <a-input placeholder="请输入FBA号" v-model="queryParam.fbaId"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="工作号">
                <a-input placeholder="请输入工作号" v-model="queryParam.jobId"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="服务">
                <j-search-select-tag placeholder="请选择服务" v-model="queryParam.service" dict="zm_service,name,name"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="目的港">
                <a-input placeholder="请输入目的港" v-model="queryParam.destination"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="仓库代码">
                <j-search-select-tag placeholder="请选择仓库代码" v-model="queryParam.warehouseId" dict="zm_fba_warehouse_detail,code,code"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="收件人">
                <a-input placeholder="请输入收件人" v-model="queryParam.recipient"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="承运商">
                <j-search-select-tag placeholder="请选择承运商" v-model="queryParam.carrier" dict="zm_partner,company,company"/>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="跟踪号">
                <a-input placeholder="请输入跟踪号" v-model="queryParam.trackingNumber"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="20" :lg="11" :md="12" :sm="24">
              <a-form-item label="计费时间">
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择开始时间" class="query-group-cust" v-model="queryParam.billableTime_begin"></j-date>
                <span class="query-group-split-cust"></span>
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择结束时间" class="query-group-cust" v-model="queryParam.billableTime_end"></j-date>
              </a-form-item>
            </a-col>
            <a-col :xl="20" :lg="11" :md="12" :sm="24" >
              <a-form-item label="出货时间">
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择开始时间" class="query-group-cust" v-model="queryParam.deliveryTime_begin"></j-date>
                <span class="query-group-split-cust"></span>
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择结束时间" class="query-group-cust" v-model="queryParam.deliveryTime_end"></j-date>
              </a-form-item>
            </a-col>
            <a-col :xl="20" :lg="11" :md="12" :sm="24">
              <a-form-item label="签收时间">
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择开始时间" class="query-group-cust" v-model="queryParam.submissionTime_begin"></j-date>
                <span class="query-group-split-cust"></span>
                <j-date :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" placeholder="请选择结束时间" class="query-group-cust" v-model="queryParam.submissionTime_end"></j-date>
              </a-form-item>
            </a-col>
          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
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


    <!-- 操作按钮区域 begin -->
    <div class="table-operator">
      <a-button type="primary" icon="plus" @click="handleAdd">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('运单')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <a-button type="primary" icon="import" @click="handleImportXls">导入</a-button>
      <waybill-import-modal ref="waybillImportModal"
                            :url="getImportUrl()"
                            @ok="importOk"

      ></waybill-import-modal>

      <!-- 高级查询区域 -->
      <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel">
            <a-icon type="delete"/>
            <span>删除</span>
          </a-menu-item>
        </a-menu>
        <a-button>
          <span>批量操作</span>
          <a-icon type="down"/>
        </a-button>
      </a-dropdown>
    </div>
    <!-- 操作按钮区域 end -->

    <!-- table区域 begin -->
    <div>

      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
        <!--        自定义列-->
        <span style="float:right;">
          <a @click="loadData()"><a-icon type="sync" />刷新</a>
          <a-divider type="vertical" />
          <a-popover title="自定义列" trigger="click" placement="leftBottom">
            <template slot="content">
              <a-checkbox-group @change="onColSettingsChange" v-model="settingColumns" :defaultValue="settingColumns">
                <a-row style="width: 400px">
                  <template v-for="(item,index) in columns">
                    <template v-if="item.key!='rowIndex'&& item.dataIndex!='action'">
                        <a-col :span="12"><a-checkbox :value="item.dataIndex"><j-ellipsis :value="item.title" :length="10"></j-ellipsis></a-checkbox></a-col>
                    </template>
                  </template>
                </a-row>
              </a-checkbox-group>
            </template>
            <a><a-icon type="setting" />设置</a>
          </a-popover>
        </span>
        <!--        自定义列 -->
      </div>
      <!-- 查询区域 -->
      <div class="table-page-search-wrapper">
        <template>
          <div>
            <a-tabs default-active-key="已下单"  type="card" @change="tabClick">

              <a-tab-pane key="已下单"       >
                <template slot="tab">
                  <p>{{ sortNum[0] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="login" @click="picking">按客户数据拣货</a-button>
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button type="danger" @click="cancel" key='0' > <a-icon type="close" />取消</a-button>
                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key="已收货"  force-render>
                <template slot="tab">
                  <p>{{ sortNum[1] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1" @click='showDeliveryModel'>
                            <a-icon type="cloud-download"/>放入出货单
                          </a-menu-item>
                          <a-menu-item key="2" @click='showAddToBill'>
                            <a-icon type="cloud-download" />
                            放入提单
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 配货  </a-button>
                      </a-dropdown>
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="stop"  @click="rejection">退件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay"  icon="cloud-download">
                          <a-menu-item key="1" @click="toStatus(1)">
                            <a-icon type="vertical-align-top" />转运中
                          </a-menu-item>
                          <a-menu-item key="2" @click="toStatus(5)">
                            <a-icon type="vertical-align-top" />已下单
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="issues-close"/> 状态调整  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button icon="plus-circle">入仓</a-button>

                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key="转运中"  >
                <template slot="tab">
                  <p>{{ sortNum[2] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <!--                      <a-dropdown >-->
                      <!--                        <a-menu slot="overlay" @click="" icon="cloud-download">-->
                      <!--                          <a-menu-item key="1">-->
                      <!--                            <a-icon type="cloud-download"/>放入出货单-->
                      <!--                          </a-menu-item>-->
                      <!--                          <a-menu-item key="2">-->
                      <!--                            <a-icon type="cloud-download"/>放入提单-->
                      <!--                          </a-menu-item>-->
                      <!--                        </a-menu>-->
                      <!--                        <a-button><a-icon type="cloud-download"/> 配货  </a-button>-->
                      <!--                      </a-dropdown>-->
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="stop"  @click="rejection">退件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1" @click='toStatus(2)'>
                            <a-icon type="vertical-align-top" />已签收
                          </a-menu-item>
                          <a-menu-item key="2" @click='toStatus(4)'>
                            <a-icon type="vertical-align-top" />已收货
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="issues-close"/> 状态调整  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button icon="plus-circle">入仓</a-button>

                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key="已签收"  >
                <template slot="tab">
                  <p>{{ sortNum[3] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"/>放入出货单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download" />放入提单
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 配货  </a-button>
                      </a-dropdown>
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="stop" @click="rejection">退件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1"  @click='toStatus(3)'>
                            <a-icon type="vertical-align-top"/>转运中
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="issues-close"/> 状态调整  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button icon="plus-circle">入仓</a-button>

                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key="退件"    >
                <template slot="tab">
                  <p>{{ sortNum[4] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="redo" @click="redeliver">重新发货</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button icon="plus-circle">入仓</a-button>

                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key="已取消"  >
                <template slot="tab">
                  <p>{{ sortNum[5] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="redo" @click="redeliver">重新发货</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button icon="plus-circle">入仓</a-button>

                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key=""   >
                <template slot="tab">
                  <p>{{ sortNum[6] }}</p>
                </template>
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="redo" @click="redeliver">重新发货</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="edit">
                          <a-menu-item key="1">
                            <a-icon type="edit"/>服务
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="edit"/>供应商服务
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="edit"/>备注
                          </a-menu-item>
                          <a-menu-item key="4" >
                            <a-icon type="edit"/>内部备注
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="edit"/> 批量修改  </a-button>
                      </a-dropdown>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>货箱
                          </a-menu-item>
                          <a-menu-item key="3" >
                            <a-icon type="cloud-download"/>申报信息
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                      </a-dropdown>
                      <a-button icon="plus-circle">入仓</a-button>

                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>

            </a-tabs>
          </div>
        </template>

      </div>
      <!-- 查询区域-END -->
      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        class="j-table-force-nowrap"
        :scroll="{x:true}"
        :loading="loading"
        :columns="defColumns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :rowSelection="{selectedRowKeys, onChange: onSelectChange}"
        @expand="handleExpand"
        @change="handleTableChange"
      >

        <!-- 内嵌table区域 begin -->
        <!--        <template slot="expandedRowRender" slot-scope="record">-->
        <!--          <a-tabs tabPosition="top">-->
        <!--            <a-tab-pane tab="货柜详情" key="zmGoodCase" forceRender>-->
        <!--              <zm-good-case-sub-table :record="record"/>-->
        <!--            </a-tab-pane>-->
        <!--          </a-tabs>-->
        <!--        </template>-->
        <!-- 内嵌table区域 end -->
        /** 超链接 */
        <a slot="httpurl" slot-scope="text">{{ text }}</a>
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

        <template slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>
          <a-divider type="vertical"/>
          <a-dropdown>
            <a class="ant-dropdown-link">
              <span>更多 <a-icon type="down"/></span>
            </a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a @click="handleDetail(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>

            </a-menu>
          </a-dropdown>

        </template>

      </a-table>
      <a-drawer
        title="详细信息"
        placement="right"
        width = "1200"
        :closable="false"
        :visible="visible"
        :after-visible-change="afterVisibleChange"
        @close="onClose"
      >
        <!--        <dataDetails> </dataDetails>-->
        <Advanced :myProp="testData" @fatherFn="getSortNum"></Advanced>

      </a-drawer>
    </div>
    <!-- table区域 end -->
    <!--加入提单-->
    <div>
      <a-modal v-model="visibleAddtoBill" title="确认提单"  @ok="submitAddtoBill">
        <a-form-model ref="form" :model="formData" >
          <!--  JDate -->
          <a-row  >
            <a-col   >
              <a-form-model-item label="柜号"  >
                <a-select
                  v-decorator="[
                      'select',
                      { rules: [{ required: true, message: '请选择提单！' }] },
                  ]"
                  placeholder="请选择提单！"
                  v-model='formData.ids'>
                  <a-select-option v-for='(item,index) in billList' :key='index' :value='item.id'>
                    {{item.billnum }}  发往  {{item.sendSite}}
                  </a-select-option>
                </a-select>
              </a-form-model-item>
            </a-col>
          </a-row>
          <!--  运单号 -->
          <a-row >
            <a-col   >
              <a-form-model-item label="运单号" >
                <div v-for='(item,index) in wayBillNo '>
                  {{item.orderId}}
                </div>
              </a-form-model-item>
            </a-col>
          </a-row>
        </a-form-model>
      </a-modal>
    </div>
    <!--加入出货单-->
    <div>
      <a-modal v-model="visibleAddtoDelivery" title="确认提单"  @ok="submitAddtoDelivery">
        <a-form-model ref="form" :model="deliveryData" >
          <!--  JDate -->
          <a-row  >
            <a-col   >
              <a-form-model-item label="柜号"  >
                <a-select
                  v-decorator="[
                      'select',
                      { rules: [{ required: true, message: '请选择出货单！' }] },
                  ]"
                  placeholder="请选择出货单！"
                  v-model='deliveryData.id'>
                  <a-select-option v-for='(item,index) in deliveryList' :key='index' :value='item.id'>
                    {{item.deliveryOrderNo }} 预计 {{item.pickingTime}} 从 {{item.departure}} 发往  {{item.destination}}
                  </a-select-option>
                </a-select>
              </a-form-model-item>
            </a-col>
          </a-row>
          <!--  运单号 -->
          <a-row >
            <a-col   >
              <a-form-model-item label="运单号" >
                <div v-for='(item,index) in wayBillNo '>
                  {{item.orderId}}
                </div>
              </a-form-model-item>
            </a-col>
          </a-row>
        </a-form-model>
      </a-modal>
    </div>
    <!-- 表单区域 -->
    <zm-waybills-modal ref="modalForm" @ok="modalFormOk"/>

  </a-card>
<!--  <a-alert-->
<!--    message="信息提示"-->
<!--    description="请在个人信息页提交认证资料"-->
<!--    type="info"-->
<!--    v-if='identity==0'-->
<!--    show-icon-->
<!--  />-->
</div>




</template>

<script>
  import Advanced from './Advanced'
  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import ZmWaybillsModal from './modules/ZmWaybillsModal'
  import WaybillImportModal from './ImportModal'
  import ZmGoodCaseSubTable from './subTables/ZmGoodCaseSubTable'
  import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import '@/assets/less/TableExpand.less'
  import  Vue  from 'vue'
  import JCodeEditor from './../../../components/jeecg/JCodeEditor'
  import { deleteAction, getAction, handleDetailss, putAction, uploadAction } from '../../../api/manage'
  import {mapGetters} from 'vuex'



  export default {
    name: 'ZmWaybillsList',
    mixins: [JeecgListMixin],
    components: {
      ZmWaybillsModal,
      ZmGoodCaseSubTable,
      Advanced,
      JCodeEditor,
      WaybillImportModal
    },
    data() {
      return {
        disableMixinCreated:true,
        form: this.$form.createForm(this),
        formItemLayout: {
          labelCol: { span: 6 },
          wrapperCol: { span: 14 },
        },
        uploading: false,
        importForm:"/zmexpress/zmWaybills/import",
        upData:{},
        userNameUpload:"",
        fileList: [],
        visibleImport:false,
        //放入出货单可见性
        visibleAddtoDelivery:false,
        //提单列表
        billList:{},
        //运单列表
        deliveryList:{},
        //分类统计
        sortNum:[],
        //运单号
        wayBillNo:"",
        //addtoBill
        formData:{
          ids:"",
          id:"",
        },
        //出货单提交数据
        deliveryData:{
          ids:"",
          deliveryId:""
        },
        //加入提单弹窗的可见性
        visibleAddtoBill: false,
        visibles: false,
        defaultActiveKey: "已下单",
        description: '运单全表列表管理页面',
        // 表头
        columns: [

          {
            title: '运单号',
            align: 'center',
            dataIndex: 'waybillId',
          },
          {
            title: '客户订单号',
            align: 'center',
            dataIndex: 'orderId',
            scopedSlots: { customRender: 'httpurl' },
            customCell:this.cellClick
          },
          {
            title: '柜号',
            align: 'center',
            dataIndex: 'caseId',
          },
          {
            title: '用户',
            align: 'center',
            dataIndex: 'createBy',
          },
          {
            title: '客户',
            align: 'center',
            dataIndex: 'username_dictText'
          },
          {
            title: '服务',
            align: 'center',
            dataIndex: 'service_dictText'
          },
          {
            title: '供应商',
            align: 'center',
            dataIndex: 'supplier',
          },
          {
            title: '供应商服务',
            align: 'center',
            dataIndex: 'supplierService',
          },
          {
            title: 'Amazon Reference ID',
            align: 'center',
            dataIndex: 'amazonReferenceId',
          },
          {
            title: '收件人',
            align: 'center',
            dataIndex: 'recipient',
          },
          {
            title: '公司名',
            align: 'center',
            dataIndex: 'company',
          },
          {
            title: '收件人国家',
            align: 'center',
            dataIndex: 'recipientCountry',
          },
          {
            title: '件数',
            align: 'center',
            dataIndex: 'pieces',
          },
          {
            title: '主品名',
            align: 'center',
            dataIndex: 'mainProductName',
          },
          {
            title: '收费重量(KG)',
            align: 'center',
            dataIndex: 'weightCharge',
          },
          {
            title: '实际重量(KG)',
            align: 'center',
            dataIndex: 'weightActual',
          },
          {
            title: '材积重(KG)',
            align: 'center',
            dataIndex: 'weightVolume',
          },
          {
            title: '体积(M³)',
            align: 'center',
            dataIndex: 'volume',
          },
          {
            title: '客户重量(KG)',
            align: 'center',
            dataIndex: 'weightCustomer',
          },
          {
            title: '客户长度(CM)',
            align: 'center',
            dataIndex: 'lengthCustomer',
          },
          {
            title: '客户宽度(CM)',
            align: 'center',
            dataIndex: 'widthCustomer',
          },
          {
            title: '客户高度(CM)',
            align: 'center',
            dataIndex: 'heightCustomer',
          },
          {
            title: '计泡系数',
            align: 'center',
            dataIndex: 'foamingFactor',
          },
          {
            title: '申报价值',
            align: 'center',
            dataIndex: 'declaredValue',
          },
          {
            title: '报关方式',
            align: 'center',
            dataIndex: 'customsDeclaration_dictText'
          },
          {
            title: '清关方式',
            align: 'center',
            dataIndex: 'customsClearance_dictText'
          },
          {
            title: '交货条款',
            align: 'center',
            dataIndex: 'termsDelivery_dictText'
          },
          {
            title: '交税方式',
            align: 'center',
            dataIndex: 'taxPayment_dictText'
          },
          {
            title: 'VAT号',
            align: 'center',
            dataIndex: 'vat',
          },
          {
            title: '属性',
            align: 'center',
            dataIndex: 'attributes_dictText'
          },
          {
            title: '已付款',
            align: 'center',
            dataIndex: 'paid',
          },
          {
            title: '状态',
            align: 'center',
            dataIndex: 'status',
          },
          {
            title: '问题件',
            align: 'center',
            dataIndex: 'problem',
          },
          {
            title: '客服代表',
            align: 'center',
            dataIndex: 'deputyCustomer',
          },
          {
            title: '销售代表',
            align: 'center',
            dataIndex: 'deputySale',
          },
          {
            title: '财务代表',
            align: 'center',
            dataIndex: 'deputyFinancial',
          },
          {
            title: '所在站点',
            align: 'center',
            dataIndex: 'site',
          },
          {
            title: '拣货站点',
            align: 'center',
            dataIndex: 'pickingSite',
          },
          {
            title: '自定义标识',
            align: 'center',
            dataIndex: 'customLogo',
          },
          {
            title: '物品属性',
            align: 'center',
            dataIndex: 'itemAttributes',
          },
          {
            title: '内部备注',
            align: 'center',
            dataIndex: 'inRemark',
          },
          {
            title: '备注',
            align: 'center',
            dataIndex: 'remark',
          },
          {
            title: '退件原因',
            align: 'center',
            dataIndex: 'reasonReturn',
          },
          {
            title: '承运商',
            align: 'center',
            dataIndex: 'carrier',
          },
          {
            title: '跟踪号',
            align: 'center',
            dataIndex: 'trackingNumber',
          },
          {
            title: '最后路由',
            align: 'center',
            dataIndex: 'finalRoute',
          },
          {
            title: '计费时间',
            align: 'center',
            dataIndex: 'timeBillable',
          },
          {
            title: '签收时间',
            align: 'center',
            dataIndex: 'timeSubmission',
          },
          {
            title: '拣货时间',
            align: 'center',
            dataIndex: 'timePicking',
          },
          {
            title: '出货时间',
            align: 'center',
            dataIndex: 'timeDelivery',
          },
          {
            title: '下单时间',
            align: 'center',
            dataIndex: 'timeOrder',
          },
          {
            title: '操作',
            dataIndex: 'action',
            align: 'center',
            fixed:"right",
            width:147,
            scopedSlots: { customRender: 'action' },
          },
        ],
        testData:{},
        visible: false,
        // 字典选项
        dictOptions: {},
        // 展开的行test
        expandedRowKeys: [],
        identity : "",
        url: {
          list: '/zmexpress/zmWaybills/list',
          delete: '/zmexpress/zmWaybills/delete',
          deleteBatch: '/zmexpress/zmWaybills/deleteBatch',
          exportXlsUrl: '/zmexpress/zmWaybills/exportXls',
          importExcelUrl: '/zmexpress/zmWaybills/importExcelSingle',
          importExcel: '/zmexpress/zmWaybills/importExcel',

        },
        superFieldList:[],
        defColumns:[],
        //列设置
        settingColumns:[],
        userInfos:{},
      }
    },
    created() {
      this.getUserInfo(this.userInfo().id);
      this.tabClick("已下单");
      this.initColumns();
      if(this.$route.query.id!=null){
         this.queryParam.waybillId = this.$route.query.id;
      }
      this.getSortNum();

      // this.getSuperFieldList();
    },
    computed: {
      importExcelUrl() {
        return window._CONFIG['domianURL'] + this.url.importExcelUrl
      },
      importExcel() {
        return window._CONFIG['domianURL'] + this.url.importExcel
      }
    },
    methods: {
      ...mapGetters(["nickname", "avatar","userInfo","userIdentity"]),
      afterVisibleChange(val) {
      },
      showDrawer() {
        this.visible = true;
      },
      onClose() {
        this.visible = false;
        this.visibles = false;
      },
      tabClick(val){
        this.queryParam.status=val;
        this.loadData(1);
      },
      /**
       * 获取用户信息
       */
      async getUserInfo(id) {
        var list = '/sys/user/queryById'
        this.userInfos =await handleDetailss(list, { id:id }).then(res => {
          if (res.success) {
            return res.result;
          }
        })
        this.identity = this.userInfos.userIdentity;
        if (this.identity==0){
          this.$message.warning(
            '请在个人信息页提交认证资料',
            5,
          );
        }
      },
      showAddToBill(){
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
        }
        this.visibleAddtoBill = true;
        this.getBillDetails();
        this.getWayBillList(ids);
      },
      /** 文件上传 */
      handleOk(e) {
        this.visibleImport = false;
        this.fileList = [];

      },
      handleImportXls(){
        this.$refs.waybillImportModal.show()
      },
      getImportUrl(){
        return '/zmexpress/zmWaybills/import'
      },
      importOk(){
        this.getSortNum()
        this.loadData(1)
      },

      showModal() {
        this.visibleImport = true;
      },
      handleRemove(file) {
        //  移除文件

        this.$message.warning(`移除当前${file.name}运单，请重新选择运单上传！`);
        const index = this.fileList.indexOf(file);
        const newFileList = this.fileList.slice();
        newFileList.splice(index, 1);
        this.fileList = newFileList;
      },
      beforeUpload(file) {
        this.fileList = [...this.fileList, file];
        return false;
      },
      handleSubmit(e) {
        e.preventDefault();
        this.form.validateFields((err, values) => {
          if (!err) {
            // console.log('Received values of form: ', values);
            // this.handleImportExcelAndForm(values);
            //
            // JSON.stringify(values);
            // const formData = new FormData();
            // values.file.forEach(file => {
            //   formData.append('files[]', file);
            // });
            // formData.append("name",values.name)
            this.uploading = true;

            console.log(values)
            // You can use any AJAX library you like

            // reqwest({
            //   url: this.importForm,
            //   method: 'post',
            //   processData: false,
            //   data: formData,
            //   success: () => {
            //     this.fileList = [];
            //     this.uploading = false;
            //     this.$message.success('upload successfully.');
            //   },
            //   error: () => {
            //     this.uploading = false;
            //     this.$message.error('upload failed.');
            //   },
            // });
          }
        });
      },
      handleUpload() {

      },
      normFile(e) {
        console.log('Upload event:', e);
        if (Array.isArray(e)) {
          return e;
        }
        return e && e.fileList;
      },
      handleCancel(e) {
        this.visibleImport = false;
      },
      initDictConfig() {
      },
      /**列设置更改事件*/
      onColSettingsChange (checkedValues) {
        var key = this.$route.name+":colsettings";
        Vue.ls.set(key, checkedValues, 7 * 24 * 60 * 60 * 1000)
        this.settingColumns = checkedValues;
        const cols = this.columns.filter(item => {
          if(item.key =='rowIndex'|| item.dataIndex=='action'){
            return true
          }
          if (this.settingColumns.includes(item.dataIndex)) {
            return true
          }
          return false
        })
        this.defColumns =  cols;
      },
      initColumns(){
        //权限过滤（列权限控制时打开，修改第二个参数为授权码前缀）
        //this.defColumns = colAuthFilter(this.defColumns,'testdemo:');
        var key = this.$route.name+":colsettings";
        let colSettings= Vue.ls.get(key);
        if(colSettings==null||colSettings==undefined){
          let allSettingColumns = [];
          this.columns.forEach(function (item,i,array ) {
            allSettingColumns.push(item.dataIndex);
          })
          this.settingColumns = allSettingColumns;
          this.defColumns = this.columns;
        }else{
          this.settingColumns = colSettings;
          const cols = this.columns.filter(item => {
            if(item.key =='rowIndex'|| item.dataIndex=='action'){
              return true;
            }
            if (colSettings.includes(item.dataIndex)) {
              return true;
            }
            return false;
          })
          this.defColumns =  cols;
        }
      },
      /** 显示出货单弹窗*/
      showDeliveryModel:function(){
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
          this.visibleAddtoDelivery = true;
          this.getWayBillList(ids);
          this.getDeliveryDetails();
        }

      },
      /** 添加到出货单*/
      submitAddtoDelivery:function(){
        var wayBillId = "";
        for (var a = 0; a < this.selectedRowKeys.length; a++) {
          wayBillId += this.selectedRowKeys[a] + ",";
        }
        var that = this;
        this.deliveryData.ids = wayBillId;
        putAction('/zmexpress/zmWaybills/inToShip',that.deliveryData).then((res) => {
          if (res.success) {
            that.reCalculatePage(that.selectedRowKeys.length)
            that.$message.success(res.message);
            that.visibleAddtoDelivery = false;
            that.loadData();
            that.onClearSelected();
            that.getSortNum();
          } else {
            that.$message.warning(res.message);
          }
        }).finally(() => {
          that.loading = false;
        });
      },

      /** 添加到提单*/
      submitAddtoBill: function() {
          var wayBillId = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            wayBillId += this.selectedRowKeys[a] + ",";
          }
          var that = this;
         this.formData.id = wayBillId;
          putAction('/zmexpress/zmWaybills/inToLading',that.formData).then((res) => {
            if (res.success) {
              that.reCalculatePage(that.selectedRowKeys.length)
              that.$message.success(res.message);
              that.visibleAddtoBill = false;
              that.getSortNum();
              that.loadData();
              that.onClearSelected();
            } else {
              that.$message.warning(res.message);
            }
          }).finally(() => {
            that.loading = false;
          });

      },
      /** 分类统计*/
      async getSortNum(){
        var list = '/zmexpress/zmWaybills/statisticsByStatus'
        var that = this;

        this.sortNum =await handleDetailss(list, "").then(res => {
          if (res.success) {
            that.reCalculatePage(that.selectedRowKeys.length)
            return res.result;
          }
        })
      },
      /** 获取所有可用提单*/
      async getBillDetails(){
        var list = '/zmexpress/zmBillloading/queryList'
        this.billList =await handleDetailss(list, "").then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 获取所有可用出货单*/
      async getDeliveryDetails(){
        var list = '/zmexpress/zmDeliveryOrder/queryByStatus'
        this.deliveryList =await handleDetailss(list, "").then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 获取所有运单*/
      async getWayBillList(ids){
          var list = '/zmexpress/zmWaybills/queryListById'
          this.wayBillNo = await handleDetailss(list, { ids:ids }).then(res => {
            if (res.success) {
              return res.result;
            }
          })
      },
      /** 切换状态 */
      toStatus: function(val) {
        // this.visibles = true
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
          var that = this;
          var hint = "";
          var title = "";
          switch (val) {
            case 1:
              hint = "请确认是否转运!";
              title = "转运";
              break;
            case 2:
              hint = "请确认是否签收！";
              title = "签收";
              break;
            case 3:
              hint = "请确认是否退回转运中？";
              title = "退回上一状态";
              val = 1;
              break;
            case 4:
              hint = "请确认是否退回已收货？";
              title = "退回上一状态";
              val= 0 ;
              break;
            case 5:
              hint = "请确认是否退回已下单";
              title = "退回上一状态";

              break;
            case 6:
              hint = "请确认是否恢复该运单！";
              title ="恢复";
              break;
          }
          this.$confirm({
            title: title,
            content: hint,
            onOk: function() {
              that.loading = true;
              let zmReason = {
                ids:ids,
                status:val
              }
              putAction(  '/zmexpress/zmWaybills/toStatus',zmReason).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.getSortNum();
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.loadData();
                  that.getSortNum();
                  that.onClearSelected();
                  that.$message.success(res.message);
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      /** 取消*/
      cancel: function() {
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');

          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
          var that = this;
          this.$confirm({
            title: "确认取消？",
            content: "是否取消选中订单?",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmWaybills/cancel',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                  that.getSortNum();
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      /** 拣货*/
      picking: function() {
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
          var that = this;
          this.$confirm({
            title: "按客户数据拣货？",
            content: "是否按客户数据拣货?",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmWaybills/picking',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                  that.getSortNum();
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      /** 退件 */
      rejection: function() {
        // this.visibles = true
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
          }
          var that = this;
          this.$JPrompt({
            title: "退件",
            content: "请输入退件原因：",
            onOk: function(value) {
              that.loading = true;
              let zmReason = {
                ids:ids,
                remark:value.value
              }
              putAction(  '/zmexpress/zmWaybills/rejection',zmReason).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                  that.getSortNum();
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },

      /** 重新发货*/
      redeliver: function() {
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
            console.log(ids);
          }
          var that = this;
          this.$confirm({
            title: "重新发货？",
            content: "是否重启选中订单?",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmWaybills/redeliver',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                  that.getSortNum();
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      /**转运*/
      transfer:function() {
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
            console.log(ids);
          }
          var that = this;
          this.$confirm({
            title: "状态调整",
            content: "是否将运单状态调整为转运中？",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmWaybills/transfer',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.getSortNum();
                  that.$message.success(res.message);
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      batchDel: function () {
        if(!this.url.deleteBatch){
          this.$message.error("请设置url.deleteBatch属性!")
          return
        }
        if (this.selectedRowKeys.length <= 0) {
          this.$message.warning('请选择一条记录！');
          return;
        } else {
          var ids = "";
          for (var a = 0; a < this.selectedRowKeys.length; a++) {
            ids += this.selectedRowKeys[a] + ",";
            console.log(ids);
          }
          var that = this;
          this.$confirm({
            title: "确认删除",
            content: "是否删除选中数据?",
            onOk: function () {
              that.loading = true;
              deleteAction(that.url.deleteBatch, {ids: ids}).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  this.getSortNum();
                  that.reCalculatePage(that.selectedRowKeys.length)
                  that.$message.success(res.message);
                  that.loadData();
                  that.onClearSelected();
                } else {
                  that.$message.warning(res.message);
                }
              }).finally(() => {
                that.loading = false;
              });
            }
          });
        }
      },
      handleExpand(expanded, record) {
        this.expandedRowKeys = []
        if (expanded === true) {
          this.expandedRowKeys.push(record.id)
        }
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'createBy',text:'创建人',dictCode:''})
        fieldList.push({type:'string',value:'waybillId',text:'运单号',dictCode:''})
        fieldList.push({type:'string',value:'orderId',text:'客户订单号',dictCode:''})
        fieldList.push({type:'string',value:'caseId',text:'柜号',dictCode:''})
        fieldList.push({type:'string',value:'referenceNumberOne',text:'参考号一',dictCode:''})
        fieldList.push({type:'string',value:'referenceNumberTwo',text:'参考号二',dictCode:''})
        fieldList.push({type:'string',value:'username',text:'用户名',dictCode:'zm_client_main,username,username'})
        fieldList.push({type:'string',value:'service',text:'服务',dictCode:'zm_service,name,name'})
        fieldList.push({type:'string',value:'supplier',text:'供应商',dictCode:''})
        fieldList.push({type:'string',value:'supplierService',text:'供应商服务',dictCode:''})
        fieldList.push({type:'string',value:'shop',text:'店铺',dictCode:''})
        fieldList.push({type:'string',value:'amazonReferenceId',text:'Amazon Reference ID',dictCode:''})
        fieldList.push({type:'string',value:'recipient',text:'收件人',dictCode:''})
        fieldList.push({type:'string',value:'company',text:'公司名',dictCode:''})
        fieldList.push({type:'string',value:'warehouseId',text:'仓库代码',dictCode:'zm_fba_warehouse_detail,code,code'})
        fieldList.push({type:'string',value:'recipientAddressOne',text:'收件人地址一',dictCode:''})
        fieldList.push({type:'string',value:'recipientAddressTwo',text:'收件人地址二',dictCode:''})
        fieldList.push({type:'string',value:'recipientAddressTri',text:'收件人地址三',dictCode:''})
        fieldList.push({type:'string',value:'recipientCity',text:'收件人城市',dictCode:''})
        fieldList.push({type:'string',value:'recipientState',text:'收件人省/州',dictCode:''})
        fieldList.push({type:'string',value:'recipientZipOde',text:'收件人邮编',dictCode:''})
        fieldList.push({type:'string',value:'recipientCountry',text:'收件人国家',dictCode:''})
        fieldList.push({type:'string',value:'recipientTel',text:'收件人电话',dictCode:''})
        fieldList.push({type:'string',value:'recipientEmail',text:'收件人邮箱',dictCode:''})
        fieldList.push({type:'string',value:'pieces',text:'件数',dictCode:''})
        fieldList.push({type:'string',value:'mainProductName',text:'主品名',dictCode:''})
        fieldList.push({type:'string',value:'weightCharge',text:'收费重量(KG)',dictCode:''})
        fieldList.push({type:'string',value:'weightActual',text:'实际重量(KG)',dictCode:''})
        fieldList.push({type:'string',value:'weightVolume',text:'材积重(KG)',dictCode:''})
        fieldList.push({type:'string',value:'volume',text:'体积(M³)',dictCode:''})
        fieldList.push({type:'string',value:'weightCustomer',text:'客户重量(KG)',dictCode:''})
        fieldList.push({type:'string',value:'lengthCustomer',text:'客户长度(CM)',dictCode:''})
        fieldList.push({type:'string',value:'widthCustomer',text:'客户宽度(CM)',dictCode:''})
        fieldList.push({type:'string',value:'heightCustomer',text:'客户高度(CM)',dictCode:''})
        fieldList.push({type:'string',value:'foamingFactor',text:'计泡系数',dictCode:''})
        fieldList.push({type:'string',value:'declaredValue',text:'申报价值',dictCode:''})
        fieldList.push({type:'string',value:'customsDeclaration',text:'报关方式',dictCode:'customs_eclaration'})
        fieldList.push({type:'string',value:'customsClearance',text:'清关方式',dictCode:'customs_clearance'})
        fieldList.push({type:'string',value:'termsDelivery',text:'交货条款',dictCode:'terms_of_delivery'})
        fieldList.push({type:'string',value:'taxPayment',text:'交税方式',dictCode:'tax_payment'})
        fieldList.push({type:'string',value:'vat',text:'VAT号',dictCode:''})
        fieldList.push({type:'string',value:'attributes',text:'属性',dictCode:'item_properties'})
        fieldList.push({type:'string',value:'paid',text:'已付款',dictCode:''})
        fieldList.push({type:'string',value:'status',text:'状态',dictCode:''})
        fieldList.push({type:'string',value:'problem',text:'问题件',dictCode:''})
        fieldList.push({type:'string',value:'deputyCustomer',text:'客服代表',dictCode:''})
        fieldList.push({type:'string',value:'deputySale',text:'销售代表',dictCode:''})
        fieldList.push({type:'string',value:'deputyFinancial',text:'财务代表',dictCode:''})
        fieldList.push({type:'string',value:'site',text:'所在站点',dictCode:''})
        fieldList.push({type:'string',value:'pickingSite',text:'拣货站点',dictCode:''})
        fieldList.push({type:'string',value:'customLogo',text:'自定义标识',dictCode:''})
        fieldList.push({type:'string',value:'itemAttributes',text:'物品属性',dictCode:''})
        fieldList.push({type:'string',value:'inRemark',text:'内部备注',dictCode:''})
        fieldList.push({type:'string',value:'remark',text:'备注',dictCode:''})
        fieldList.push({type:'string',value:'reasonReturn',text:'退件原因',dictCode:''})
        fieldList.push({type:'string',value:'carrier',text:'承运商',dictCode:''})
        fieldList.push({type:'string',value:'trackingNumber',text:'跟踪号',dictCode:''})
        fieldList.push({type:'string',value:'finalRoute',text:'最后路由',dictCode:''})
        fieldList.push({type:'date',value:'timeBillable',text:'计费时间'})
        fieldList.push({type:'date',value:'timeSubmission',text:'签收时间'})
        fieldList.push({type:'date',value:'timePicking',text:'拣货时间'})
        fieldList.push({type:'date',value:'timeDelivery',text:'出货时间'})
        fieldList.push({type:'date',value:'timeOrder',text:'下单时间'})
        this.superFieldList = fieldList
      },
      /** 单元格点击事件*/
      cellClick(record) {
        return {
          style: {
            // 'color': 'blue', //这里将名称变了下色
          },
          on: {
            click: () => { //点击事件，也可以加其他事件
              // this.$router.push({path: '/profile/advanced',query:{record: record}})
              this.testData=record;
              // this.receiveItem(record);
              this.showDrawer();
              // this.getCaseDetails(record);
            }
          }
        }
      },
    }
  }
</script>

<style lang="less" scoped>
  @import '~@assets/less/common.less';
  #components-form-demo-validate-other .dropbox {
    height: 180px;
    line-height: 1.5;
  }
</style>