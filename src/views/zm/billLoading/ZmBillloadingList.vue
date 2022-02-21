<template>
  <div>
    <a-card :bordered="false" v-show='identity!=0'>
      <!-- 查询区域 -->
      <div class="table-page-search-wrapper">
        <a-form layout="inline" @keyup.enter.native="searchQuery">
          <a-row :gutter="24">
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="柜号">
                <a-input placeholder="请输入柜号" v-model="queryParam.billnum"></a-input>
              </a-form-item>
            </a-col>
            <a-col :xl="6" :lg="7" :md="8" :sm="24">
              <a-form-item label="提单号">
                <a-input placeholder="请输入提单号" v-model="queryParam.inBillnum"></a-input>
              </a-form-item>
            </a-col>
            <template v-if="toggleSearchStatus">
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="类型">
                  <j-dict-select-tag placeholder="请选择类型" v-model="queryParam.type" dictCode="bill_loding"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="供应商">
                  <j-search-select-tag placeholder="请选择供应商" v-model="queryParam.supplier" dict="zm_supplier  ,company,company"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="承运商">
                  <j-search-select-tag placeholder="请选择承运商" v-model="queryParam.arriage" dict="zm_supplier,company,company"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="清关公司">
                  <j-search-select-tag placeholder="请选择清关公司" v-model="queryParam.customsClearance" dict="zm_supplier,company,company"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="出发站点">
                  <j-search-select-tag placeholder="请选择出发站点" v-model="queryParam.departurePoint" dict="zm_airport,name,name"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="发往站点">
                  <j-search-select-tag placeholder="请选择发往站点" v-model="queryParam.sendSite" dict="zm_airport,name,name"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="集装箱规格">
                  <j-dict-select-tag placeholder="请选择集装箱规格" v-model="queryParam.container" dictCode="container"/>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="班次">
                  <a-input placeholder="请输入班次" v-model="queryParam.shift"></a-input>
                </a-form-item>
              </a-col>
              <a-col :xl="6" :lg="7" :md="8" :sm="24">
                <a-form-item label="船号">
                  <a-input placeholder="请输入船号" v-model="queryParam.shipNumber"></a-input>
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


      <!-- 操作按钮区域 -->
      <div class="table-operator">
        <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
        <a-button type="primary" icon="download" @click="handleExportXls('提单表')">导出</a-button>
        <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
          <a-button type="primary" icon="import">导入</a-button>
        </a-upload>
        <!-- 高级查询区域 -->
        <j-super-query :fieldList="superFieldList" ref="superQueryModal" @handleSuperQuery="handleSuperQuery"></j-super-query>
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
        <div>
          <a-tabs default-active-key="0"  type="card" @change="changTab" v-model="queryParam.status" >
            <a-tab-pane key="待订仓" :tab="sortNum[0]" >
              <template >
                <div  >
                  <a-button-group  >
                    <a-button icon="check"  @click='toStatus(1)'>已定仓</a-button>
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay"   icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>
                    <a-button icon="delete" type="danger" @click='toCancel'>作废</a-button>
                    <a-button icon="delete" type="danger">删除</a-button>

                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="已定仓" :tab="sortNum[1]" force-render>
              <template >
                <div  >
                  <a-button-group    >
                    <a-button icon="car"  @click='toStatus(2)'>已出发</a-button>
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay"  icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>
                    <a-button icon="delete" type="danger" @click='toCancel'>作废</a-button>
                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="已出发" :tab="sortNum[2]">
              <template >
                <div  >
                  <a-button-group >
                    <a-button icon="bank"  @click='toStatus(3)'>已到站</a-button>
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay"  icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>
                    <a-button icon="delete" type="danger" @click='toCancel'>作废</a-button>

                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="已到站" :tab="sortNum[3]">
              <template >
                <div  >
                  <a-button-group   >
                    <a-button icon="like" @click='toStatus(4)'>已清关</a-button>
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay"  icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>
                    <a-button icon="delete" type="danger" @click='toCancel'>作废</a-button>

                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="已清关" :tab="sortNum[4]">
              <template >
                <div  >
                  <a-button-group  >
                    <a-button icon="like"  @click='toStatus(5)'>已完成</a-button>
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay" icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>
                    <a-button icon="delete" type="danger" @click='toCancel'>作废</a-button>

                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="已完成" :tab="sortNum[5]"  >
              <template >
                <div  >
                  <a-button-group >
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay"  icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>

                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="已作废" :tab="sortNum[6]" >
              <template >
                <div  >
                  <a-button-group >
                    <a-button icon="issues-close">审计</a-button>
                    <a-button icon="redo">反审计</a-button>
                    <a-button icon="redo">更新统计</a-button>

                    <a-dropdown >
                      <a-menu slot="overlay"  icon="cloud-download">
                        <a-menu-item key="1">
                          <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>运单
                        </a-menu-item>
                        <a-menu-item key="2">
                          <a-icon type="cloud-download"/>提单
                        </a-menu-item>
                        <a-menu-item key="3" >
                          <a-icon type="cloud-download"/>运单
                        </a-menu-item>
                        <a-menu-item key="4" >
                          <a-icon type="cloud-download"/>货箱
                        </a-menu-item>
                        <a-menu-item key="5" >
                          <a-icon type="cloud-download"/>申报
                        </a-menu-item>
                        <a-menu-item key="6" >
                          <a-icon type="cloud-download"/>流水
                        </a-menu-item>
                      </a-menu>
                      <a-button><a-icon type="cloud-download"/> 导出  </a-button>
                    </a-dropdown>
                    <a-button  @click='toStatus(6)' ><a-icon type="cloud-sync" />恢复</a-button>

                  </a-button-group>
                </div>
              </template>
            </a-tab-pane>
            <a-tab-pane key="" :tab="sortNum[7]">
            </a-tab-pane>
          </a-tabs>
        </div>
        <a-table
          ref="table"
          size="middle"
          :scroll="{x:true}"
          bordered
          rowKey="id"
          :columns="defColumns"
          :dataSource="dataSource"
          :pagination="ipagination"
          :loading="loading"
          :rowSelection="{selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
          class="j-table-force-nowrap"
          @change="handleTableChange">
          /** 超链接 */
          <a slot="httpurl" slot-scope="text">{{ text }}</a>
          <!--        自定义列-->
          <div slot="filterDropdown">
            <a-card>
              <a-checkbox-group @change="onColSettingsChange" v-model="settingColumns" :defaultValue="settingColumns">
                <a-row style="width: 400px">
                  <template v-for="(item,index) in columns">
                    <template v-if="item.key!='rowIndex'&& item.dataIndex!='action'">
                      <a-col :span="12"><a-checkbox :value="item.dataIndex"><j-ellipsis :value="item.title" :length="10"></j-ellipsis></a-checkbox></a-col>
                    </template>
                  </template>
                </a-row>
              </a-checkbox-group>
            </a-card>
          </div>
          <a-icon slot="filterIcon" type='setting' :style="{ fontSize:'16px',color:  '#108ee9' }" />
          <!--        自定义列-->
          <template slot="htmlSlot" slot-scope="text">
            <div v-html="text"></div>
          </template>
          <template slot="imgSlot" slot-scope="text">
            <span v-if="!text" style="font-size: 12px;font-style: italic;">无图片</span>
            <img v-else :src="getImgView(text)" height="25px" alt="" style="max-width:80px;font-size: 12px;font-style: italic;"/>
          </template>
          <template slot="fileSlot" slot-scope="text">
            <span v-if="!text" style="font-size: 12px;font-style: italic;">无文件</span>
            <a-button
              v-else
              :ghost="true"
              type="primary"
              icon="download"
              size="small"
              @click="downloadFile(text)">
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
                <a @click="handleBillDetails(record)">详情</a>
              </a-menu-item>
              <a-menu-item>
                <a @click="">test</a>
              </a-menu-item>
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
      <div >
        <a-drawer
          placement="right"
          :width="1280"
          :closable="false"
          :visible="visible"
          :after-visible-change="afterVisibleChange"
          @close="onClose"
        >
          <a-card :bordered="false" :title="title">
            <div style="position:absolute ;right: 50px ;top: 10px" >
              <a-button-group    style="margin-right: 20px" >
                <a-button icon="printer" @click='handleExportExcel'>打印标签</a-button>
                <a-button icon="edit" @click="handleEditBill()">修改</a-button>
                <a-dropdown >
                  <a-menu slot="overlay"  icon="cloud-download">
                    <a-menu-item key="1">
                      <a-icon type="cloud-download"  @click="handleExportXls('导入fba表')"/>导出发票
                    </a-menu-item>
                    <a-menu-item key="2">
                      <a-icon type="cloud-download"/>导出运单
                    </a-menu-item>
                    <a-menu-item key="3" >
                      <a-icon type="cloud-download"/>申报信息
                    </a-menu-item>
                  </a-menu>
                  <a-button><a-icon type="cloud-download"/> 下载  </a-button>
                </a-dropdown>
              </a-button-group>
            </div>

            <a-row>
              <a-col :span="16">
                <a-collapse v-model="activeKey" >
                  <a-collapse-panel key="1" header="基础信息" >
                    <detail-list >
                      <detail-list-item term="类型">{{billDtail.type_dictText}}</detail-list-item>
                      <detail-list-item term="提单号">{{billDtail.billnum}}</detail-list-item>
                      <detail-list-item term="柜号">{{billDtail.billnum}}</detail-list-item>
                      <detail-list-item term="船名">{{billDtail.shipNumber}}</detail-list-item>
                      <detail-list-item term="航次">{{billDtail.shift}}</detail-list-item>
                      <detail-list-item term="集装箱规格">{{billDtail.container_dictText}}</detail-list-item>
                      <detail-list-item term="供应商">{{billDtail.supplier}}</detail-list-item>
                      <detail-list-item term="船公司">{{billDtail.arriage}}</detail-list-item>
                      <detail-list-item term="出发地址">{{billDtail.departurePoint_dictText}}</detail-list-item>
                      <detail-list-item term="发往地点">{{billDtail.departurePoint_dictText}}</detail-list-item>
                      <detail-list-item term="出发国家">{{billDtail.billnum}}</detail-list-item>
                      <detail-list-item term="发往国家">{{billDtail.billnum}}</detail-list-item>
                      <detail-list-item term="报关代理">{{billDtail.declarationCompany}}</detail-list-item>
                      <detail-list-item term="清关代理">{{billDtail.customsClearance_dictText}}</detail-list-item>
                      <!--                    <detail-list-item term="单独清关">{{billDtail.billnum}}</detail-list-item>-->
                      <!--                    <detail-list-item term="参考号">{{billDtail.billnum}}</detail-list-item>-->
                      <detail-list-item term="工作号">{{billDtail.workNo}}</detail-list-item>
                      <detail-list-item term="出发时间">{{billDtail.departure}}</detail-list-item>
                      <detail-list-item term="到达时间">{{billDtail.arrive}}</detail-list-item>
                      <detail-list-item term="清关时间">{{billDtail.customsClearanceTime}}</detail-list-item>
                      <detail-list-item term="备注">{{billDtail.remark}}</detail-list-item>
                      <detail-list-item term="内部备注">{{billDtail.inRemark}}</detail-list-item>
                      <detail-list-item term="装柜顺序">{{billDtail.billnum}}</detail-list-item>
                      <detail-list-item term="提单创建时间">{{billDtail.createTime}}</detail-list-item>

                    </detail-list>
                  </a-collapse-panel>
                </a-collapse>
              </a-col>
              <a-col :span="8">
                <a-collapse v-model="routeKey" >
                  <a-collapse-panel key="1" header="路由信息" >
                    <a-timeline  mode="alternate" >
                      <a-timeline-item v-for="(item,index) in  logDetail"  :key='index'>
                        <a-icon slot="dot" type="down-circle" style="font-size: 16px;"  @click='deleteMsg(item.id)'/>
                        {{ item.msg }}
                        <label v-if='item.remark!=""'><br/>备注：{{item.remark}}</label>
                        <br/>{{ item.createTime }}
                      </a-timeline-item>

                      <a-tooltip placement="bottom" >
                        <template slot="title"  >
                          添加路由信息
                        </template>
                        <a-timeline-item color="red">
                          <a-icon slot="dot" type="clock-circle-o" style="font-size: 16px;"  @click='showMsg'/>
                        </a-timeline-item>
                      </a-tooltip>
                    </a-timeline>
                  </a-collapse-panel>
                </a-collapse>

              </a-col>
            </a-row>
            <a-divider style="margin-bottom: 32px"/>
            <div >

              <a-row :gutter="24">
                <a-col :span="7">
                  <a-card title="运单" style="width: 300px">
                    <a slot="extra" href="#" @click="showWaybills">查看运单</a>
                    <a-statistic title="运单数量"  :value='statisticsCase.waybillCount ' style="margin-right: 50px"  :value-style="{ fontSize: '40px' }" />

                  </a-card>
                </a-col>
                <a-col :span="16">
                  <a-card title="货箱" style="width: 820px">
                    <a slot="extra"  href="#"  >添加货箱   | </a>
                    <a slot="extra" href="#" @click='showCases'>查看货箱</a>
                    <a-row >
                      <a-col :span="6">
                        <a-statistic title="货箱数量" :value='statisticsCase.caseCount' style="margin-right: 50px"  :value-style="{ fontSize: '40px' }" />
                      </a-col>
                      <a-col :span="6">
                        <a-statistic title="实重(KG)" :value='statisticsCase.weight' style="margin-right: 50px"  :value-style="{ fontSize: '40px' }" />
                      </a-col>
                      <a-col :span="6">
                        <a-statistic title="材积重(KG)" :value='statisticsCase.volumeWeight' style="margin-right: 50px"  :value-style="{ fontSize: '40px' }"/>
                      </a-col>
                      <a-col :span="6">
                        <a-statistic title="总体积(m³)" :value='statisticsCase.volume' style="margin-right: 50px"   :value-style="{ fontSize: '40px' }"/>
                      </a-col>
                    </a-row>
                  </a-card>
                </a-col>
              </a-row>
            </div>
            <a-divider style="margin-bottom: 32px"/>
          </a-card>
          <zm-bill-waybill-detail ref='waybillDetail'
                                  :billDtail='billDtail'
                                  @getNewStatistics='getStatisticsCase'
          ></zm-bill-waybill-detail>
          <case-list v-show='visibleCaseList'
                     :visibleCaseList.sync="visibleCaseList"
                     :billDetail='billDtail'
                     @getNewStatisticsCase='getStatisticsCase'
          ></case-list>

        </a-drawer>
      </div>

      <!--添加路由信息-->
      <div>
        <a-modal v-model="visibleLogistics" title="路由信息"  @ok="submitMsg">
          <a-form-model ref="form" :model="formData" >
            <!--  JDate -->
            <a-row  >
              <a-col   >
                <a-form-model-item label="时间" >
                  <j-date v-model="formData.jDate" :showTime="true" dateFormat="YYYY-MM-DD HH:mm:ss"/>
                </a-form-model-item>
              </a-col>
            </a-row>
            <!--  操作信息 -->
            <a-row >
              <a-col   >
                <a-form-model-item label="操作信息" >
                  <a-input  v-model="formData.msg" placeholder="请输入操作信息" />
                </a-form-model-item>
              </a-col>
            </a-row>

            <!--  备注 -->
            <a-row  >
              <a-col  :md="24" :sm="24">
                <a-form-model-item label="备注" >
                  <a-textarea
                    v-model="formData.remark"
                    placeholder="请输入备注"
                    :auto-size="{ minRows: 3, maxRows: 5 }"
                  />
                </a-form-model-item>
              </a-col>
            </a-row>

          </a-form-model>
        </a-modal>
      </div>


      <zm-billloading-modal ref="modalForm" @ok="modalFormOk" ></zm-billloading-modal>
      <zm-logistics-information-modal ref="modalLogisticeMsg" @ok="modalFormOk" ></zm-logistics-information-modal>
    </a-card>

  </div>

</template>

<script>

  import '@/assets/less/TableExpand.less'
  import { mixinDevice } from '@/utils/mixin'
  import { JeecgListMixin } from '../../../mixins/JeecgListMixin'
  import ZmBillloadingModal from './modules/ZmBillloadingModal__Style#Drawer'
  import ZmBillloadingDetail from './modules/billLoadingDetail'
  import Vue from 'vue'
  import PageLayout from '@/components/page/PageLayout'
  import STable from '@/components/table/'
  import DetailList from '@/components/tools/DetailList'
  import ABadge from "ant-design-vue/es/badge/Badge"
  import ZmLogisticsInformationModal from './modules/ZmLogisticsInformationModal'
  import { deleteAction, handleDetailss, putAction, getAction, downFile } from '../../../api/manage'
  import ZmLogisticsInformationForm from './modules/ZmLogisticsInformationForm'
  import ZmLogisticsAdd from './modules/ZmLogisticsAdd'
  import ZmBillWaybillDetail from './modules/ZmBillWaybillDetail'
  const DetailListItem = DetailList.Item
  import JDate from '@/components/jeecg/JDate'
  import { VueAxios } from "@/utils/request"
  import CaseList from './caseList'
  import {mapGetters} from 'vuex'
  export default {
    name: 'ZmBillloadingList',
    mixins:[JeecgListMixin, mixinDevice],
    components: {
      CaseList,
      ZmLogisticsAdd,
      ZmLogisticsInformationForm,
      ZmBillloadingModal,
      ZmBillWaybillDetail,
      ZmLogisticsInformationModal,
      PageLayout,
      ABadge,
      DetailList,
      DetailListItem,
      STable,
      JDate
    },
    data () {
      return {
        billDtail:{},
        description: '提单表管理页面',
        defColumns:[],
        //列设置
        settingColumns:[],
        visibleLogistics: false,
        visible: false,

        //默认打开得折叠面板
        activeKey:['1'],

        //路由信息
        logDetail:{},
        //路由抽屉
        routeKey:"1",
        // 查询参数
        queryParam: {},
        //运单数量
        waybillsNum:"",
        //控制运单列表显示
        isShowWaybill:false,
        visibleCaseList:false,
        identity : "",
        // 表头
        columns: [

          {
            title:'柜号',
            align:"center",
            dataIndex: 'billnum',
            scopedSlots: { customRender: 'httpurl' },
            customCell:this.cellClick
          },
          {
            title:'提单号',
            align:"center",
            dataIndex: 'inBillnum',

          },
          {
            title:'类型',
            align:"center",
            dataIndex: 'type_dictText'
          },
          {
            title:'供应商',
            align:"center",
            dataIndex: 'supplier_dictText'
          },
          {
            title:'船公司',
            align:"center",
            dataIndex: 'arriage_dictText'
          },
          {
            title:'清关公司',
            align:"center",
            dataIndex: 'customsClearance_dictText'
          },
          {
            title:'出发站点',
            align:"center",
            dataIndex: 'departurePoint_dictText'
          },
          {
            title:'发往站点',
            align:"center",
            dataIndex: 'sendSite_dictText'
          },
          {
            title:'箱数',
            align:"center",
            dataIndex: 'boxes'
          },
          {
            title:'合箱数',
            align:"center",
            dataIndex: 'allBoxes'
          },
          {
            title:'价值',
            align:"center",
            dataIndex: 'worth'
          },
          {
            title:'实重',
            align:"center",
            dataIndex: 'weight'
          },
          {
            title:'体积比',
            align:"center",
            dataIndex: 'volumeRatio'
          },
          {
            title:'应收',
            align:"center",
            dataIndex: 'receivable'
          },
          {
            title:'提单应付',
            align:"center",
            dataIndex: 'billLadingPayable'
          },
          {
            title:'运单应付',
            align:"center",
            dataIndex: 'waybillPayable'
          },
          {
            title:'体积',
            align:"center",
            dataIndex: 'volume'
          },
          {
            title:'盈亏',
            align:"center",
            dataIndex: 'profit'
          },
          {
            title:'备注',
            align:"center",
            dataIndex: 'remark',
            ellipsis: true,
          },
          {
            title:'内部备注',
            align:"center",
            dataIndex: 'inRemark',
            ellipsis: true,
          },
          {
            title:'集装箱规格',
            align:"center",
            dataIndex: 'container'
          },
          {
            title:'班次',
            align:"center",
            dataIndex: 'shift'
          },
          {
            title:'船号',
            align:"center",
            dataIndex: 'shipNumber'
          },
          {
            title:'出发时间',
            align:"center",
            dataIndex: 'departure'
          },
          {
            title:'到达时间',
            align:"center",
            dataIndex: 'arrive'
          },
          {
            title:'清关时间',
            align:"center",
            dataIndex: 'customsClearanceTime'
          },
          {
            title:'创建时间',
            align:"center",
            dataIndex: 'creatTime'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: {
              filterDropdown: 'filterDropdown',
              filterIcon: 'filterIcon',
              customRender: 'action'},
          }
        ],
        goodsColumns: [
          {
            title: '商品编号',
            dataIndex: 'id',
            key: 'id'
          },
          {
            title: '商品名称',
            dataIndex: 'name',
            key: 'name'
          },
          {
            title: '商品条码',
            dataIndex: 'barcode',
            key: 'barcode'
          },
          {
            title: '单价',
            dataIndex: 'price',
            key: 'price',
            align: 'right'
          },
          {
            title: '数量（件）',
            dataIndex: 'num',
            key: 'num',
            align: 'right'
          },
          {
            title: '金额',
            dataIndex: 'amount',
            key: 'amount',
            align: 'right'
          }
        ],
        url: {
          list: "/zmexpress/zmBillloading/list",
          delete: "/zmexpress/zmBillloading/delete",
          deleteBatch: "/zmexpress/zmBillloading/deleteBatch",
          exportXlsUrl: "/zmexpress/zmBillloading/exportXls",
          importExcelUrl: "zmexpress/zmBillloading/importExcel",
          msg: "/zmexpress/zmLogisticsInformation/queryById",
        },
        dictOptions:{},
        superFieldList:[],
        // 加载数据方法 必须为 Promise 对象
        loadGoodsData: () => {
          return new Promise((resolve => {
            resolve({
              data: this.dataList,
              pageSize: 10,
              pageNo: 1,
              totalPage: 1,
              totalCount: 10
            })
          })).then(res => {
            return res
          })
        },

        scheduleColumns: [
          {
            title: '时间',
            dataIndex: 'createTime',
            key: 'createTime'
          },
          {
            title:'物流信息',
            align:"center",
            dataIndex: 'msg',
            scopedSlots: { customRender: 'msg' },
          },
          {
            title:'备注',
            align:"center",
            dataIndex: 'remark',
          },
          {
            title: '操作员',
            dataIndex: 'createBy',
            key: 'createBy',
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            fixed:"right",
            width:147,
            scopedSlots: {
              customRender: 'action'},
          }
        ],
        loadScheduleData: () => {
          return new Promise((resolve => {
            resolve({
              data: this.dataList,
              pageSize: 10,
              pageNo: 1,
              totalPage: 1,
              totalCount: 10
            })
          })).then(res => {
            return res
          })
        },
        statisticsCase: {},
        sortNum:[],
        dataList: [],
        formData: {
          jDate: this.getCurrentTime(),
          remark:'',
          msg:'',
          orderId:'',
        },
        userInfos:{},
      }
    },

    created() {
     // this.getSuperFieldList();
      this.getUserInfo(this.userInfo().id);
      this.initColumns();
      this.getSortNum();
    },
    filters: {
      statusFilter(status) {
        const statusMap = {
          'processing': '进行中',
          'success': '完成',
          'failed': '失败'
        }
        return statusMap[status]
      }
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      },
      title () {
        var title = this.billDtail.billnum + '/' +this.billDtail.arriage + '/' ;
        return title;
      }
    },

    methods: {
      ...mapGetters(["nickname", "avatar","userInfo"]),

      afterVisibleChange(val) {
      },
      showDrawer() {
        this.visible = true;
      },
      onClose() {
        this.visible = false;
      },
      showCases(){
        this.visibleCaseList = true;
      },
      closeAddlogisticsMsg(){
        this.visibleLogistics = false;
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
      handleExportExcel(fileName){
        if(!fileName || typeof fileName != "string"){
          fileName = "导出文件"
        }
        console.log("导出")
        downFile("/zmexpress/zmBillloading/exportCustomsInformation", { billId:this.billDtail.id }).then((data)=>{
          if (!data) {
            this.$message.warning("文件下载失败")
            return
          }
          if (typeof window.navigator.msSaveBlob !== 'undefined') {
            window.navigator.msSaveBlob(new Blob([data],{type: 'application/vnd.ms-excel'}), this.dataFba+'.xls')
          }else{
            let url = window.URL.createObjectURL(new Blob([data],{type: 'application/vnd.ms-excel'}))
            let link = document.createElement('a')
            link.style.display = 'none'
            link.href = url
            link.setAttribute('download', this.billDtail.billnum+'报关资料.xls')
            document.body.appendChild(link)
            link.click()
            document.body.removeChild(link); //下载完成移除元素
            window.URL.revokeObjectURL(url); //释放掉blob对象
          }
        })
      },
      /** 统计货箱*/
      async getStatisticsCase(){
        this.statisticsCase=await handleDetailss( '/zmexpress/zmBillloading/statistic', {id:this.billDtail.id}).then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      /** 分类统计*/
      async getSortNum(){
        this.sortNum =await handleDetailss('/zmexpress/zmBillloading/statisticsByStatus', "").then(res => {
          if (res.success) {
            return res.result;
          }
        })
      },
      showMsg(){
        this.getLogDetails();
        this.visibleLogistics=true;
      },
      /** 打开运单列表*/
      showWaybills(){
        this.$refs.waybillDetail.showDrawer();
      },
      deleteMsg: function(id) {
        var  myDate  = new Date();
        var  that = this;
        this.$confirm({
          title: '警告',
          content: '真的要删除吗?',
          okText: '删除',
          okType: 'danger',
          cancelText: '取消',
          onOk() {
            deleteAction(  '/zmexpress/zmLogisticsInformation/delete',{id:id}).then((res) => {
              if (res.success) {
                that.$message.success(res.message);
                that.getLogDetails();
              } else {
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.loading = false;
            });
          },
          onCancel() {
            console.log('Cancel');
          },
        });

      },

      /** 物流信息方法*/
      handleChange (value, key, column) {
        console.log(value, key, column)
      },
      edit (row) {
        row.editable = true
        // row = Object.assign({}, row)
        this.$refs.tableLog.updateEdit()
      },
      // eslint-disable-next-line
      del :function(row) {
        var that = this;
        this.$confirm({
          title: '警告',
          content: '真的要删除吗?',
          okText: '删除',
          okType: 'danger',
          cancelText: '取消',
          onOk() {
            deleteAction('/zmexpress/zmLogisticsInformation/delete',{id:row.id}).then((res) => {
              if (res.success) {
                that.$message.success(res.message);
                that.getLogDetails();
                that.loadData();
                that.onClearSelected();
              } else {
                that.$message.warning(res.message);
              }
            });
          },
          onCancel() {
            console.log('Cancel');
          },
        });
      },
      save (row) {
        delete row.editable
        this.$refs.tableLog.updateEdit()
      },
      cancel (row) {
        delete row.editable
        this.$refs.tableLog.updateEdit()
      },

      onChange (row) {
        this.selectedRowKeys = row.selectedRowKeys
        this.selectedRows = row.selectedRows
      },
      /** 获取提单中的运单数量*/
      getWaybillNum(){
        var  that  = this ;
        let  id = this.billDtail.id;
        getAction('/zmexpress/zmWaybills/getTotalWaybillByBill', { id: id}).then((res) => {
          if (res.success) {
            //重新计算分页问题
            that.waybillsNum = res.result;
          } else {
            that.$message.warning(res.message);
          }
        }).finally(() => {
          that.loading = false;
        });
      },
      /** 编辑提单 */
      handleEditBill(){
        this.handleEdit(this.billDtail);
      },
      /** 添加物流信息 */
      submitMsg: function() {
        var  myDate  = new Date();
        var  that = this;
        this.formData.orderId = this.billDtail.id;
        putAction(  '/zmexpress/zmLogisticsInformation/addMsg',this.formData).then((res) => {
          if (res.success) {
            that.$message.success(res.message);
            that.getLogDetails();
          } else {
            that.$message.warning(res.message);
          }
        }).finally(() => {
          that.loading = false;
        });
        setTimeout(() => {
          this.visibleLogistics = false;
        }, 20);
        this.formData={
          jDate: this.getCurrentTime(),
          remark:'',
          msg:'',
          orderId:this.billDtail.id,
        };
      },
      async getLogDetails(){
        let params = this.billDtail.id
        this.logDetail =await handleDetailss('/zmexpress/zmLogisticsInformation/queryByOrderId', {id: params}).then(res => {
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
              hint = "请确认是否订仓!";
              title = "订仓";
              break;
            case 2:
              hint = "请确认是否出发！";
              title = "出发";
              break;
            case 3:
              hint = "请确认是否到达！";
              title = "到达";
              break;
            case 4:
              hint = "请确认是否清关！";
              title = "清关";
              break;
            case 5:
              hint = "请确认是否完成！";
              title = "完成";
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
              putAction(  '/zmexpress/zmBillloading/tostatus',zmReason).then((res) => {
                if (res.success) {
                  //重新计算分页问题
                  that.getSortNum();
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
      /** 作废 */
      toCancel: function() {
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
          this.$JPrompt({
            title: "作废",
            content: "请输入作废原因：",
            onOk: function(value) {
              that.loading = true;
              let zmReason = {
                ids:ids,
                remark:value.value
              }
              putAction(  '/zmexpress/zmBillloading/toCancel',zmReason).then((res) => {
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
      //单元格点击事件
      cellClick(record) {
        return {
          style: {
            // 'color': 'blue', //这里将名称变了下色
          },
          on: {
            click: () => { //点击事件，也可以加其他事件
              this.billDtail = record;
              this.getWaybillNum();
              this.getLogDetails();
              this.showDrawer();
              this.getStatisticsCase();
            }
          }
        }
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
      initDictConfig(){
      },
      getSuperFieldList(){
        let fieldList=[];
        fieldList.push({type:'string',value:'billnum',text:'提单号',dictCode:''})
        fieldList.push({type:'string',value:'type',text:'类型',dictCode:''})
        fieldList.push({type:'string',value:'supplier',text:'供应商',dictCode:''})
        fieldList.push({type:'string',value:'arriage',text:'承运商',dictCode:''})
        fieldList.push({type:'string',value:'customsClearance',text:'清关公司',dictCode:''})
        fieldList.push({type:'string',value:'line',text:'路线',dictCode:''})
        fieldList.push({type:'string',value:'user',text:'用户',dictCode:''})
        fieldList.push({type:'int',value:'boxes',text:'箱数',dictCode:''})
        fieldList.push({type:'int',value:'allBoxes',text:'合箱数',dictCode:''})
        fieldList.push({type:'string',value:'weight',text:'实重',dictCode:''})
        fieldList.push({type:'string',value:'materialWeight',text:'材重',dictCode:''})
        fieldList.push({type:'string',value:'heavyCharge',text:'收费重',dictCode:''})
        fieldList.push({type:'string',value:'volume',text:'体积',dictCode:''})
        fieldList.push({type:'string',value:'profit',text:'盈亏',dictCode:''})
        fieldList.push({type:'string',value:'remark',text:'备注',dictCode:''})
        fieldList.push({type:'string',value:'inRemark',text:'内部备注',dictCode:''})
        fieldList.push({type:'date',value:'departure',text:'出发时间'})
        fieldList.push({type:'date',value:'arrive',text:'到达时间'})
        fieldList.push({type:'date',value:'customsClearanceTime',text:'清关时间'})
        this.superFieldList = fieldList
      }
    },
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
  .title {
    color: rgba(0,0,0,.85);
    font-size: 16px;
    font-weight: 500;
    margin-bottom: 16px;
  }
  .my_drawer {
    width: 500px;
  }
  .card-avatar {
    width: 48px;
    height: 48px;
    border-radius: 48px;
  }


  .card-avatar {
    width: 48px;
    height: 48px;
    border-radius: 48px;
  }

  .ant-card-actions {
    background: #f7f9fa;
  li {
    float: left;
    text-align: center;
    margin: 12px 0;
    color: rgba(0, 0, 0, 0.45);
    width: 50%;

  &:not(:last-child) {
     border-right: 1px solid #e8e8e8;
   }

  a {
    color: rgba(0, 0, 0, .45);
    line-height: 22px;
    display: inline-block;
    width: 100%;
  &:hover {
     color: #1890ff;
   }
  }
  }
  }

  .new-btn {
    background-color: #fff;
    border-radius: 2px;
    width: 100%;
    height: 186px;
  }

  .meta-content {
    position: relative;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    height: 64px;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
  }

  .ant-card-actions {
    background: #f7f9fa;
  li {
    float: left;
    text-align: center;
    margin: 12px 0;
    color: rgba(0, 0, 0, 0.45);
    width: 50%;

  &:not(:last-child) {
     border-right: 1px solid #e8e8e8;
   }

  a {
    color: rgba(0, 0, 0, .45);
    line-height: 22px;
    display: inline-block;
    width: 100%;
     &:hover {
     color: #1890ff;
      }
    }
   }
  }

  .new-btn {
    background-color: #fff;
    border-radius: 2px;
    width: 100%;
    height: 186px;
  }

  .meta-content {
    position: relative;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    height: 64px;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
  }
</style>