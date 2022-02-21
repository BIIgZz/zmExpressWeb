<template>
  <a-card class="j-inner-table-wrapper" :bordered="false">

    <!-- 查询区域 begin -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="24">
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="FBA ID">
             <a-input v-model="queryParam.fbaid" placeholder="请输入FBA ID"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="客户订单号">
             <a-input v-model="queryParam.orderid" placeholder="请输入客户订单号"/>
            </a-form-item>
          </a-col>
          <template v-if="toggleSearchStatus">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="服务">
            <j-dict-select-tag v-model="queryParam.serviceId" placeholder="请选择服务" dictCode="zm_service,name,name"/>
              </a-form-item>
            </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="地址库编码">
             <j-search-select-tag placeholder="请选择地址库编码" v-model="queryParam.code" dict="zm_fba_warehouse_detail,code,code"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="收件人">
             <a-input v-model="queryParam.name" placeholder="请输入收件人"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="公司">
             <a-input v-model="queryParam.company" placeholder="请输入公司"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="国家">
          <j-dict-select-tag v-model="queryParam.countryCode" placeholder="请选择国家" dictCode="zm_tool_countries,cname,cname"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="物品属性">
          <j-dict-select-tag v-model="queryParam.properties" placeholder="请选择物品属性" dictCode="item_properties"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="报关方式">
          <j-dict-select-tag v-model="queryParam.customsEclaration" placeholder="请选择报关方式" dictCode="customs_eclaration"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="清关方式">
          <j-dict-select-tag v-model="queryParam.customsClearance" placeholder="请选择清关方式" dictCode="customs_clearance"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="交税方式">
          <j-dict-select-tag v-model="queryParam.taxPayment" placeholder="请选择交税方式" dictCode="tax_payment"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="交货条款">
          <j-dict-select-tag v-model="queryParam.deliveryTerm" placeholder="请选择交货条款" dictCode="terms_of_delivery"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="创建时间">
          <j-date class="query-group-cust" v-model="queryParam.createTime_begin" placeholder="请选择开始时间" show-time date-format="YYYY-MM-DD HH:mm:ss"/>
          <span class="query-group-split-cust"></span>
          <j-date class="query-group-cust" v-model="queryParam.createTime_end" placeholder="请选择结束时间" show-time date-format="YYYY-MM-DD HH:mm:ss"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="更新时间">
          <j-date class="query-group-cust" v-model="queryParam.updateTime_begin" placeholder="请选择开始时间" show-time date-format="YYYY-MM-DD HH:mm:ss"/>
          <span class="query-group-split-cust"></span>
          <j-date class="query-group-cust" v-model="queryParam.updateTime_end" placeholder="请选择结束时间" show-time date-format="YYYY-MM-DD HH:mm:ss"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="创建人">
             <a-input v-model="queryParam.createBy" placeholder="请输入创建人"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="状态">
             <a-input v-model="queryParam.status" placeholder="请输入状态"/>
            </a-form-item>
          </a-col>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <a-form-item label="客户">
             <a-input v-model="queryParam.clientid" placeholder="请输入客户"/>
            </a-form-item>
          </a-col>
          </template>
          <a-col :xl="6" :lg="7" :md="8" :sm="24">
            <span class="table-page-search-submitButtons table-operator">
              <a-button type="primary" icon="search" @click="searchQuery">查询</a-button>
              <a-button type="primary" icon="reload" @click="searchReset">重置</a-button>
              <a @click="handleToggleSearch" style="margin-left: 8px">
                <span>{{ toggleSearchStatus ? '收起' : '展开' }}</span>
                <a-icon :type="toggleSearchStatus ? 'up' : 'down'"/>
              </a>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域 end -->

    <!-- 操作按钮区域 begin -->
    <div class="table-operator">
      <a-button type="primary" icon="plus" @click="handleAdd">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('导入FBA表(已导出)')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
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

      <a-alert type="info" showIcon style="margin-bottom: 16px;">
        <template slot="message">
          <span>已选择</span>
          <a style="font-weight: 600;padding: 0 4px;">{{ selectedRowKeys.length }}</a>
          <span>项</span>
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
        </template>
      </a-alert>
      <div class="table-page-search-wrapper">
        <template>
          <div>
            <a-tabs default-active-key="0"  type="card" @change="searchQuery"     v-model="queryParam.status">
              <a-tab-pane key="" tab="全部" >
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="redo">重新发货</a-button>
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
              <a-tab-pane key="0" tab="已下单"    >
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="login" @click="">按客户数据拣货</a-button>
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
                      <a-button type="danger" @click="cancelItem" key='0' > <a-icon type="close" />取消</a-button>
                    </a-button-group>
                  </div>
                </template>
              </a-tab-pane>
              <a-tab-pane key="1" tab="已收货" force-render>
                <template>
                  <div>
                    <a-button-group >
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"/>放入出货单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>放入提单
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 配货  </a-button>
                      </a-dropdown>
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="stop">退件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="vertical-align-top" />转运中
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
              <a-tab-pane key="2" tab="转运中">
                <template>
                  <div>
                    <a-button-group >
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"/>放入出货单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>放入提单
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 配货  </a-button>
                      </a-dropdown>
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="stop">退件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="vertical-align-top" />已签收
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
              <a-tab-pane key="3" tab="已签收">
                <template>
                  <div>
                    <a-button-group >
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="cloud-download"/>放入出货单
                          </a-menu-item>
                          <a-menu-item key="2">
                            <a-icon type="cloud-download"/>放入提单
                          </a-menu-item>
                        </a-menu>
                        <a-button><a-icon type="cloud-download"/> 配货  </a-button>
                      </a-dropdown>
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="stop">退件</a-button>
                      <a-dropdown >
                        <a-menu slot="overlay" @click="" icon="cloud-download">
                          <a-menu-item key="1">
                            <a-icon type="vertical-align-top" />已签收
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
              <a-tab-pane key="4" tab="退件">
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="redo">重新发货</a-button>
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
              <a-tab-pane key="5" tab="已取消">
                <template>
                  <div>
                    <a-button-group >
                      <a-button icon="printer">打印标签</a-button>
                      <a-button icon="close-circle">拦截/问题件</a-button>
                      <a-button icon="redo">重新发货</a-button>
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
      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        class="j-table-force-nowrap"
        :scroll="{x:true}"
        :loading="loading"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :expandedRowKeys="expandedRowKeys"
        :rowSelection="{selectedRowKeys, onChange: onSelectChange}"
        @expand="handleExpand"
        @change="handleTableChange"
      >

        <!-- 内嵌table区域 begin -->
        <template slot="expandedRowRender" slot-scope="record">
          <a-tabs tabPosition="top">
            <a-tab-pane tab="货柜详情" key="zmImportGood" forceRender>
              <zm-import-good-sub-table :record="record"/>
            </a-tab-pane>
          </a-tabs>
        </template>
        <!-- 内嵌table区域 end -->

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
    </div>
    <!-- table区域 end -->

    <!-- 表单区域 -->
    <zm-import-fba-modal ref="modalForm" @ok="modalFormOk"/>

  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import ZmImportFbaModal from './modules/ZmImportFbaModal'
  import ZmImportGoodSubTable from './subTables/ZmImportGoodSubTable'
  import JDictSelectTag from '@/components/dict/JDictSelectTag.vue'
  import JDate from '@/components/jeecg/JDate.vue'
  import {filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import JSearchSelectTag from '@/components/dict/JSearchSelectTag'
  import '@/assets/less/TableExpand.less'
  import { handleDetailss, putAction } from '../../../api/manage'
  import Vue from 'vue'

  export default {
    name: 'ZmImportFbaList',
    mixins: [JeecgListMixin],
    components: {
      ZmImportFbaModal,
      ZmImportGoodSubTable,
      JDate,
      JDictSelectTag,
      JSearchSelectTag,
    },
    data() {
      return {
        description: '导入FBA表(已导出)列表管理页面',
        // 表头
        columns: [
          {
            title: '#',
            key: 'rowIndex',
            width: 60,
            align: 'center',
            customRender: (t, r, index) => parseInt(index) + 1
          },
          {
            title: 'FBA ID',
            align: 'center',
            sorter: true,
            dataIndex: 'fbaid',
          },
          {
            title: '客户订单号',
            align: 'center',
            dataIndex: 'orderid',
          },
          {
            title: '服务',
            align: 'center',
            dataIndex: 'serviceId_dictText'
          },
          {
            title: '地址库编码',
            align: 'center',
            dataIndex: 'code_dictText'
          },
          {
            title: '收件人',
            align: 'center',
            dataIndex: 'name',
          },
          {
            title: '公司',
            align: 'center',
            dataIndex: 'company',
          },
          {
            title: '地址',
            align: 'center',
            dataIndex: 'address',
          },
          {
            title: '城市',
            align: 'center',
            dataIndex: 'city',
          },
          {
            title: '州',
            align: 'center',
            dataIndex: 'province',
          },
          {
            title: '邮编',
            align: 'center',
            dataIndex: 'postcode',
          },
          {
            title: '国家',
            align: 'center',
            dataIndex: 'countryCode_dictText'
          },
          {
            title: '电话',
            align: 'center',
            dataIndex: 'tel',
          },
          {
            title: '邮箱',
            align: 'center',
            dataIndex: 'email',
          },
          {
            title: '总箱数',
            align: 'center',
            dataIndex: 'caseNumber',
          },
          {
            title: '物品属性',
            align: 'center',
            dataIndex: 'properties_dictText'
          },
          {
            title: '报关方式',
            align: 'center',
            dataIndex: 'customsEclaration_dictText'
          },
          {
            title: '清关方式',
            align: 'center',
            dataIndex: 'customsClearance_dictText'
          },
          {
            title: '交税方式',
            align: 'center',
            dataIndex: 'taxPayment_dictText'
          },
          {
            title: '交货条款',
            align: 'center',
            dataIndex: 'deliveryTerm_dictText'
          },
          {
            title: 'VAT号',
            align: 'center',
            dataIndex: 'vat',
          },
          {
            title: '参考号1',
            align: 'center',
            dataIndex: 'referenceNumber1',
          },
          {
            title: '参考号2',
            align: 'center',
            dataIndex: 'referenceNumber2',
          },
          {
            title: '备注',
            align: 'center',
            dataIndex: 'note',
          },
          {
            title: '发件人姓名',
            align: 'center',
            dataIndex: 'nameSender',
          },
          {
            title: '公司',
            align: 'center',
            dataIndex: 'companySender',
          },
          {
            title: '地址',
            align: 'center',
            dataIndex: 'addressSender',
          },
          {
            title: '城市',
            align: 'center',
            dataIndex: 'citySender',
          },
          {
            title: '省份',
            align: 'center',
            dataIndex: 'provinceSender',
          },
          {
            title: '邮编',
            align: 'center',
            dataIndex: 'postcodeSender',
          },
          {
            title: '国家',
            align: 'center',
            dataIndex: 'countryCodeSender_dictText'
          },
          {
            title: '电话',
            align: 'center',
            dataIndex: 'telSender',
          },
          {
            title: '邮箱',
            align: 'center',
            dataIndex: 'emailSender',
          },
          {
            title: '客户',
            align: 'center',
            dataIndex: 'clientid',
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
        //列设置
        settingColumns:[],
        testData:"",
        visible: false,
        defColumns:[],
        // 字典选项
        dictOptions: {},
        // 展开的行test
        expandedRowKeys: [],
        url: {
          list: '/zmexpress/zmImportFba/list',
          delete: '/zmexpress/zmImportFba/delete',
          deleteBatch: '/zmexpress/zmImportFba/deleteBatch',
          exportXlsUrl: '/zmexpress/zmImportFba/exportXls',
          importExcelUrl: '/zmexpress/zmImportFba/importExcel',
        },
        superFieldList:[],
      }
    },
    created() {
      this.getSuperFieldList();
    },
    computed: {
      importExcelUrl() {
        return window._CONFIG['domianURL'] + this.url.importExcelUrl
      }
    },
    methods: {
      initDictConfig() {
      },

      handleExpand(expanded, record) {
        this.expandedRowKeys = []
        if (expanded === true) {
          this.expandedRowKeys.push(record.id)
        }
      },
      /**收货*/
      receiveItem(record){
        console.log("收货")

        this.$http.put("/zmexpress/zmWaybill/changeStatus?id="+record.id+"&operate=2").then((response) =>{
          console.log(response);
        })
      },

      /** 单元格点击事件*/
      cellClick2(record) {
        return {
          style: {
            // 'color': 'blue', //这里将名称变了下色
          },
          on: {
            click: () => { //点击事件，也可以加其他事件
              // this.$router.push({path: '/profile/advanced',query:{record: record}})
              this.testData=record;
              this.receiveItem(record);
              this.showDrawer();
              this.getCaseDetails(record);
            }
          }
        }
      },
      async getCaseDetails(record){
        let list = '/zmexpress/zmWaybill/queryById'
        let params = record.id
        this.dataList =await handleDetailss(list, {id: params}).then(res => {
          if (res.success) {

          }
        })
      },
      //列设置更改事件
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
      cancelItem: function() {
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
            title: "确认修改",
            content: "是否修改选中数据?",
            onOk: function() {
              that.loading = true;
              putAction(  '/zmexpress/zmWaybill/cancel',{ ids: ids }).then((res) => {
                if (res.success) {
                  //重新计算分页问题
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
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'fbaid',text:'FBA ID',dictCode:''})
        fieldList.push({type:'string',value:'orderid',text:'客户订单号',dictCode:''})
        fieldList.push({type:'string',value:'serviceId',text:'服务',dictCode:'zm_service,name,name'})
        fieldList.push({type:'sel_search',value:'code',text:'地址库编码',dictTable:'zm_fba_warehouse_detail', dictText:'code', dictCode:'code'})
        fieldList.push({type:'string',value:'name',text:'收件人',dictCode:''})
        fieldList.push({type:'string',value:'company',text:'公司',dictCode:''})
        fieldList.push({type:'string',value:'address',text:'地址',dictCode:''})
        fieldList.push({type:'string',value:'city',text:'城市',dictCode:''})
        fieldList.push({type:'string',value:'province',text:'州',dictCode:''})
        fieldList.push({type:'string',value:'postcode',text:'邮编',dictCode:''})
        fieldList.push({type:'string',value:'countryCode',text:'国家',dictCode:'zm_tool_countries,cname,cname'})
        fieldList.push({type:'string',value:'tel',text:'电话',dictCode:''})
        fieldList.push({type:'string',value:'email',text:'邮箱',dictCode:''})
        fieldList.push({type:'int',value:'caseNumber',text:'总箱数',dictCode:''})
        fieldList.push({type:'string',value:'properties',text:'物品属性',dictCode:'item_properties'})
        fieldList.push({type:'int',value:'customsEclaration',text:'报关方式',dictCode:'customs_eclaration'})
        fieldList.push({type:'int',value:'customsClearance',text:'清关方式',dictCode:'customs_clearance'})
        fieldList.push({type:'int',value:'taxPayment',text:'交税方式',dictCode:'tax_payment'})
        fieldList.push({type:'string',value:'deliveryTerm',text:'交货条款',dictCode:'terms_of_delivery'})
        fieldList.push({type:'string',value:'vat',text:'VAT号',dictCode:''})
        fieldList.push({type:'string',value:'referenceNumber1',text:'参考号1',dictCode:''})
        fieldList.push({type:'string',value:'referenceNumber2',text:'参考号2',dictCode:''})
        fieldList.push({type:'string',value:'note',text:'备注',dictCode:''})
        fieldList.push({type:'string',value:'nameSender',text:'发件人姓名',dictCode:''})
        fieldList.push({type:'string',value:'companySender',text:'公司',dictCode:''})
        fieldList.push({type:'string',value:'addressSender',text:'地址',dictCode:''})
        fieldList.push({type:'string',value:'citySender',text:'城市',dictCode:''})
        fieldList.push({type:'string',value:'provinceSender',text:'省份',dictCode:''})
        fieldList.push({type:'string',value:'postcodeSender',text:'邮编',dictCode:''})
        fieldList.push({type:'string',value:'countryCodeSender',text:'国家',dictCode:'zm_tool_countries,cname,cname'})
        fieldList.push({type:'string',value:'telSender',text:'电话',dictCode:''})
        fieldList.push({type:'string',value:'emailSender',text:'邮箱',dictCode:''})
        fieldList.push({type:'datetime',value:'createTime',text:'创建时间'})
        fieldList.push({type:'datetime',value:'updateTime',text:'更新时间'})
        fieldList.push({type:'string',value:'createBy',text:'创建人',dictCode:''})
        fieldList.push({type:'string',value:'status',text:'状态',dictCode:''})
        fieldList.push({type:'string',value:'clientid',text:'客户',dictCode:''})
        this.superFieldList = fieldList
      }
    }
  }
</script>
<style lang="less" scoped>
  @import '~@assets/less/common.less';
</style>